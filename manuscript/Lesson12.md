<!-- reference: https://www.virtuability.com/public/wp/?p=12 -->
# Lesson 12 - Set up AWS IAM with Multi-Factor Authentication using AWS CLI

# Overview

One way to at least mitigate the risk of AWS keys and secrets being usable if falling into the wrong hands is to make those sets of credentials with high-impacting permissions time-limited and to make policies dependent on multi-factor authentication. Time limitation can be achieved by issuing credentials via the Secure Token Service, which was introduced in 2015.

I have worked with STS combined with MFA over the last few weeks in an effort to increase security while still enabling people to get on with their work – and I have come up with a self-service approach that involves the following example set-up:

# The Walkthrough

Create a new IAM user testuser and issue a set of API key + secret credentials – then store them in ~/.aws/credentials file (or use aws configure to do so), e.g. setting up a new profile testuser. Do not assign the user any groups or policies of any kind.
Set-up an MFA device for the user (e.g. using Google Authenticator)
Create a new IAM policy that make permissions conditional on MFA. In this case we allow the EC2 DescribeRegions call to be made – but only if the user MFA is authenticated.
```cmd
{
"Version": "2012-10-17",
"Statement": [
{
"Effect": "Allow",
"Action": "ec2:DescribeRegions",
"Resource": "*",
"Condition": {
"Bool": {
"aws:MultiFactorAuthPresent": "true"
}
}
}
]
}
```

Assign just this policy either to a group (with no other permissions) or the user; if the former, assign the group to testuser
With the previously generated API key&secret attempt to make the ec2:DescribeRegions call using the aws CLI (as expected, this will fail)

```cmd
aws ec2 describe-regions --profile testuser --region eu-west-1
```

A client error (UnauthorizedOperation) occurred when calling the DescribeRegions operation: You are not authorized to perform this operation.

Now, with the same API key/secret request an STS token using the aws CLI. In my case I created a small helper script aws-temp-creds.sh and put it in my local ~/bin/ folder to do so, which when sourced will export the resulting temporary key, secret and token to environment variables (with a default validity period of 4 hours) – make sure to replace the serial-number in the script with the actual (in this case) virtual MFA ARN of the user:

```cmd
unset AWS_ACCESS_KEY_ID AWS_SECRET_ACCESS_KEY AWS_SESSION_TOKEN
echo -n "Enter token followed by [ENTER]: "
read token

output=`aws sts get-session-token --duration-seconds 14400 --serial-number arn:aws:iam::012345678912:mfa/testuser --token-code $token --output text "$@"`
rc=$?

if [[ $rc -ne 0 ]] ; then
return $rc
fi

oarr=($output)

export AWS_ACCESS_KEY_ID="${oarr[1]}"
export AWS_SECRET_ACCESS_KEY="${oarr[3]}"
export AWS_SESSION_TOKEN="${oarr[4]}"
```

Now call the temporary script and attempt to run the ec2:DescribeRegions call:

```cmd
. aws-temp-creds.sh --profile testuser
Enter token followed by [ENTER]: 123456

aws ec2 describe-regions --region eu-west-1
REGIONS	ec2.eu-west-1.amazonaws.com	eu-west-1
REGIONS	ec2.ap-southeast-1.amazonaws.com	ap-southeast-1
REGIONS	ec2.ap-southeast-2.amazonaws.com	ap-southeast-2
REGIONS	ec2.eu-central-1.amazonaws.com	eu-central-1
REGIONS	ec2.ap-northeast-2.amazonaws.com	ap-northeast-2
REGIONS	ec2.ap-northeast-1.amazonaws.com	ap-northeast-1
REGIONS	ec2.us-east-1.amazonaws.com	us-east-1
REGIONS	ec2.sa-east-1.amazonaws.com	sa-east-1
REGIONS	ec2.us-west-1.amazonaws.com	us-west-1
REGIONS	ec2.us-west-2.amazonaws.com	us-west-2
```

Finally, to test what happens when the credentials no longer are available one could try the following:

```cmd
unset AWS_ACCESS_KEY_ID AWS_SECRET_ACCESS_KEY AWS_SESSION_TOKEN
aws ec2 describe-regions --region eu-west-1
Unable to locate credentials. You can configure credentials by running "aws configure".
aws ec2 describe-regions --region eu-west-1 --profile testuser
A client error (UnauthorizedOperation) occurred when calling the DescribeRegions operation: You are not authorized to perform this operation.
```

# What's Going On

The reason this approach works is that the environment variables take precedence over the credentials configured in ~/.aws/credentials. Therefore, if the environment variables are not set one would default back to the non-MFA permissions – currently none, although this needn’t necessarily be the case.

As for choosing a validity period of the token this would be a balance between risk and convenience. In my case I choose 4 hours as this allows me to work in the morning until lunch, then take out a new set of credentials in the afternoon. The chance of someone intercepting the temporary set of credentials and being able to use them before they expire is therefore reasonably low.

On the other hand the risk of 3rd party access to the “permanent” credentials stored in the .aws/credentials file is in this case mitigated by the fact that there are no inherent permissions assigned because the credentials are not MFA authenticated. While one should still rotate them regularly the possible impact of interception is considerably reduced.

Another advantage is that with MFA authentication in place one can unify the policies & permissions for users of both the API and the Console because two-factor authentication is enabled.

Finally, one point worth mentioning is that there are per-region soft limits to how many instances of services like RDS, EC2 etc. that can be spun up. To further reduce the likelihood of attackers managing to spin up a considerable amount of costly estate, e.g. for the purposes of bitcoin mining, one would want to remove the ability to issue STS tokens for regions that are not being used in the account. To do so, simply follow the STS blog previously mentioned and do the reverse (deactivate) regions from STS.

# Questions
