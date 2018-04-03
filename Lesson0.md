# Lesson 0 - Getting Started

## Objectives
* Sign up for AWS
* Create an IAM User
* Obtain an Access Key
* Install the AWS CLI


### Sign Up for AWS
When you sign up for Amazon Web Services (AWS), your AWS account is automatically signed up for all services in AWS. You are charged only for the services that you use. If you are a new AWS customer, you can get started with Amazon RDS for free; for more information, see AWS Free Usage Tier.

If you have an AWS account already, skip to the next task. If you don't have an AWS account, use the following procedure to create one.

### To create an AWS account

Open ```https://aws.amazon.com/```, and then choose 'Create an AWS Account'.

> Note

> This might be unavailable in your browser if you previously signed into the AWS Management Console. In that case, choose Sign In to the Console, and then choose Create a new AWS account.


Part of the sign-up procedure involves receiving a phone call and entering a PIN using the phone keypad.

> Note your AWS account number, because you'll need it for the next task.

### Create an IAM (Identity and Access Management) User

Services in AWS, such as Amazon RDS, require that you provide credentials when you access them, so that the service can determine whether you have permission to access its resources.

The console requires your password. You can create access keys for your AWS account to access the command line interface or API. However, it is recommended that you use AWS Identity and Access Management (IAM) instead.

Create an IAM user, and then add the user to an IAM group with administrative permissions or grant this user administrative permissions. You can then access AWS using a special URL and the credentials for the IAM user.

If you signed up for AWS but have not created an IAM user for yourself, you can create one using the IAM console.

#### To create an IAM user for yourself and add the user to an Administrators group

* Use your AWS account email address and password to sign in to the AWS Management Console as the AWS account root user.

* In the navigation pane of the console, choose Users, and then choose Add user.

* For User name, type Administrator.

* Select the check box next to AWS Management Console access, select Custom password, and then type the new user's password in the text box. You can optionally select Require password reset to force the user to select a new password the next time the user signs in.

* Choose Next: Permissions. On the Set permissions for user page, choose Add user to group.

* Choose Create group. In the Create group dialog box, type Administrators.

* For Filter, choose Job function.

* In the policy list, select the check box for AdministratorAccess. Then choose Create group.

* Back in the list of groups, select the check box for your new group. Choose Refresh if necessary to see the group in the list.

* Choose Next: Review to see the list of group memberships to be added to the new user. When you are ready to proceed, choose Create user.

> You can use this same process to create more groups and users, and to give your users access to your AWS account resources. To learn about using policies to restrict users' permissions to specific AWS resources, go to Access Management and Example Policies.

* To sign in as this new IAM user, sign out of the AWS console, then use the following URL, replace 123456789012 with your AWS account number (without the hyphens) ```https://123456789012.signin.aws.amazon.com/console/```

* Enter the IAM user name and password that you just created. When you're signed in, the navigation bar displays "your_user_name @ 1234-5678-9012".





## How to get an Access Key



To create, disable, or delete an access key for your AWS account root user

Use your AWS account email address and password to sign in to the AWS Management Console as the AWS account root user.

If you previously signed in to the console with IAM user credentials, your browser might remember this preference and open your account-specific sign-in page. You cannot use the IAM user sign-in page to sign in with your AWS account root user credentials. If you see the IAM user sign-in page, choose Sign-in using root account credentials near the bottom of the page to return to the main sign-in page. From there, you can type your AWS account email address and password.

On the IAM Dashboard page, choose your account name in the navigation bar, and then choose My Security Credentials.

If you see a warning about accessing the security credentials for your AWS account, choose Continue to Security Credentials.

Expand the Access keys (access key ID and secret access key) section.

Choose your preferred action:

To create an access key
Choose Create New Access Key. Then choose Download Key File to save the access key ID and secret access key to a file on your computer. After you close the dialog box, you can't retrieve this secret access key again.

To disable an existing access key
Choose Make Inactive next to the access key that you are disabling. To reenable an inactive access key, choose Make Active.

To delete an existing access key
Before you delete an access key, make sure it's no longer in use. For more information, see Finding unused access keys in the IAM User Guide. You can't recover an access key after deleting it. To delete your access key, choose Delete next to the access key that you you want to delete.


# Install the AWS CLI
The AWS Command Line Interface (CLI) is a unified tool to manage your AWS services. With just one tool to download and configure, you can control multiple AWS services from the command line and automate them through scripts.

The AWS CLI introduces a new set of simple file commands for efficient file transfers to and from Amazon S3.

Requirements: Python 2.6 or higher with pip

Run the following command on your computer:
```cmd
pip install awscli
```

## What can you do with the AWS CLI?
The AWS CLI is an open source tool built on top of the AWS SDK for Python (Boto) that provides commands for interacting with AWS services. With minimal configuration, you can start using all of the functionality provided by the AWS Management Console from your favorite terminal program.
* Linux shells – Use common shell programs such as Bash, Zsh, and tsch to run commands in Linux, macOS, or Unix.
* Windows command line – On Microsoft Windows, run commands in either PowerShell or the Windows Command Processor.
* Remotely – Run commands on Amazon EC2 instances through a remote terminal such as PuTTY or SSH, or with Amazon EC2 systems manager.

You can learn about the different commands from their site at ```http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html```
