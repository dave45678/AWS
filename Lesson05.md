# Lesson 05 - Set up EC2 with IAM user and Virtual Private Cloud

If you've already signed up for Amazon Web Services (AWS), you can start using Amazon EC2 immediately. You can open the Amazon EC2 console, choose Launch Instance, and follow the steps in the launch wizard to launch your first instance.

An instance is a virtual server in the AWS cloud. With Amazon EC2, you can set up and configure the operating system and applications that run on your instance. Once your instance is set up you can install applications. For example, you could install a LAMP server and host web applications.

Amazon Elastic Compute Cloud (Amazon EC2) is a web service that provides secure, resizable compute capacity in the cloud. It is designed to make web-scale cloud computing easier for developers. Amazon EC2's simple web service interface allows you to obtain and configure capacity with minimal friction.


### Complete the following tasks set up for Amazon EC2
1. Sign Up for AWS (Amazon Web Services)
2. Create an IAM User called Administrator with administrative access
3. Create a Key Pair so you don't need a password
4. Create a Virtual Private Cloud (VPC)
5. Create a Security Group
6. Connect to your instance
7. Terminate your instance

### Log into AWS

Browse to ```https://console.aws.amazon.com``` and use your user name and password to gain access.

### Create an IAM (Identity and Access Management) User

A User within your AWS Account is an identity with unique security credentials that can be used to access AWS Services. Just because your data can be accessed anywhere doesn't mean it should. Like usernames and passwords on the desktop, IAM allows you to place restrictions on who can get to your data. IAM (Identity and Access Management) is a feature of AWS which eliminates the need to share passwords or access keys. IAM also makes it simple to enable or disable a User's access. You can create one or multiple IAM users in your AWS account. An IAM user may be a person who belongs to your organization or it may be an application that needs to make API (Application Programming Interface) calls to your AWS.

IAM provides flexibility, control and security when using AWS.

Services in AWS, such as Amazon RDS, require that you provide credentials when you access them. This is so the service can determine whether you have permission to access its resources.

The console requires your password. You can create access keys for your AWS account to access the command line interface or API. However, it is recommended that you use AWS Identity and Access Management (IAM) instead.

Create an IAM user, and then add the user to an IAM group with administrative permissions or grant this user administrative permissions. You can then access AWS using a special URL and the credentials for the IAM user.

If you signed up for AWS but have not created an IAM user for yourself, you can create one using the IAM console.


#### To create an IAM user for yourself and add the user to an Administrators group

* Use your AWS account email address and password to sign in to the AWS Management Console as the AWS account root user.

* In the navigation pane of the console, choose Users, and then choose *Add user*.

* For User name, type *Administrator*.

* Select the check box next to *AWS Management Console* access, select *Custom password*, and then type the new user's password in the text box. You can optionally select *Require password reset* to force the user to select a new password the next time the user signs in.

* Choose *Next: Permissions*. On the Set permissions for user page, choose *Add user to group*.

* Choose *Create group*. In the *Group name* dialog box, type *Administrators*.

* For Filter, choose *Job function*.

* In the policy list, select the check box for Administrator Access. Then click the *Create group* button.

* Back in the list of groups, select the check box for your new group. Choose Refresh if necessary to see the group in the list.

* Choose *Next: Review* to see the list of group memberships to be added to the new user. When you are ready to proceed, choose *Create user*.

> You can use this same process to create more groups and users, and to give your users access to your AWS account resources. To learn about using policies to restrict users' permissions to specific AWS resources, go to Access Management and Example Policies.

* To sign in as this new IAM user, sign out of the AWS console, then use the following URL, replace 123456789012 with your AWS account number (without the hyphens) ```https://123456789012.signin.aws.amazon.com/console/```

* You will be prompted to change the password the first time you sign in with that user.

* Enter the IAM user name and password that you just created. When you're signed in, the navigation bar displays "your_user_name @ 1234-5678-9012".


### Create a Key Pair

AWS uses public-key cryptography to secure the login information for your instance. A Linux instance has no password; you use a key pair to log in to your instance securely. You specify the name of the key pair when you launch your instance, then provide the private key when you log in using SSH.

If you haven't created a key pair already, you can create one using the Amazon EC2 console. Note that if you plan to launch instances in multiple regions, you'll need to create a key pair in each region. For more information about regions, see Regions and Availability Zones.

#### To create a key pair

* Sign in to AWS using the URL that you created in the previous section.
* From the AWS dashboard, choose EC2 to open the Amazon EC2 console.
* From the navigation bar, select a region for the key pair.
> You can select any region that's available to you, regardless of your location. However, key pairs are specific to a region; for example, if you plan to launch an instance in the US East (Ohio) Region, you must create a key pair for the instance in the US East (Ohio) Region.
* Select a region
* Open the key pairs page and choose Create Key Pair.
    * Enter a name for the new key pair in the Key pair name field of the Create Key Pair dialog box
    * choose Create. Use a name that is easy for you to remember

> The private key file is automatically downloaded by your browser. The base file name is the name you specified as the name of your key pair, and the file name extension is .pem. Save the private key file in a safe place.


> Note: This is the only chance for you to save the private key file. You'll need to provide the name of your key pair when you launch an instance and the corresponding private key each time you connect to the instance.

If you will use an SSH client on a Mac or Linux computer to connect to your Linux instance, use the following command to set the permissions of your private key file so that only you can read it.

```chmod 400 your_user_name-key-pair-region_name.pem```


#### To connect to your instance using your key pair

To connect to your Linux instance from a computer running Mac or Linux, you'll specify the .pem file to your SSH client with the -i option and the path to your private key. To connect to your Linux instance from a computer running Windows, use PuTTY. If you plan to use PuTTY, you'll need to install it and convert the .pem file to a .ppk file. This is explained in the Putty guide: ```https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/putty.html```.


### Create a Virtual Private Cloud (VPC)

Amazon VPC enables you to launch AWS resources into an isolated virtual network that you've defined. If you have a default VPC, you can skip this section and move to the next task to create a Security Group. To determine whether you have a default VPC, see Supported Platforms in the Amazon EC2 Console. Otherwise, you can create a non-default VPC in your account using the steps below.

> Important: If your account supports EC2-Classic in a region, then you do not have a default VPC in that region. T2 instances must be launched into a VPC.

#### To create a non-default VPC

Open the Amazon VPC console at ```https://console.aws.amazon.com/vpc/```.

From the navigation bar, select a region for the VPC. VPCs are specific to a region, so you should select the same region in which you created your key pair.

On the VPC dashboard, choose Start VPC Wizard.

On the Step 1: Select a VPC Configuration page, ensure that VPC with a Single Public Subnet is selected, and choose Select.

On the Step 2: VPC with a Single Public Subnet page, enter a friendly name for your VPC in the VPC name field. Leave the other default configuration settings, and choose Create VPC. On the confirmation page, choose OK.


### Create a Security Group

Security groups act as a firewall for associated instances, controlling both inbound and outbound traffic at the instance level. You must add rules to a security group that enable you to connect to your instance from your IP address using SSH. You can also add rules that allow inbound and outbound HTTP and HTTPS access from anywhere.

Note that if you plan to launch instances in multiple regions, you'll need to create a security group in each region. For more information about regions, see Regions and Availability Zones.

#### Prerequisites

You'll need the public IPv4 address of your local computer. The security group editor in the Amazon EC2 console can automatically detect the public IPv4 address for you. Alternatively, you can use the search phrase "what is my IP address" in an Internet browser, or use the following service: Check IP. If you are connecting through an Internet service provider (ISP) or from behind a firewall without a static IP address, you need to find out the range of IP addresses used by client computers.

#### To create a security group with least privilege

Open the Amazon EC2 console at https://console.aws.amazon.com/ec2/.

#### Select a region

1. From the navigation bar, select a region for the security group. Security groups are specific to a region, so you should select the same region in which you created your key pair.

2. Choose Security Groups in the navigation pane.

3. Choose Create Security Group.

4. Enter a name for the new security group and a description. Use a name that is easy for you to remember, such as your IAM user name, followed by _SG_, plus the region name. For example, me_SG_useast1.

5. In the VPC list, select your VPC. If you have a default VPC, it's the one that is marked with an asterisk (*).

> Note: If your account supports EC2-Classic, select the VPC that you created in the previous task.

6. On the Inbound tab, create the following rules (choose Add Rule for each new rule), and then choose Create:

7. Choose HTTP from the Type list, and make sure that Source is set to Anywhere (0.0.0.0/0).

8. Choose SSH from the Type list.

9. In the Source box, choose My IP to automatically populate the field with the public IPv4 address of your local computer. Alternatively, choose Custom and specify the public IPv4 address of your computer or network in CIDR notation. To specify an individual IP address in CIDR notation, add the routing suffix /32, for example, 203.0.113.25/32. If your company allocates addresses from a range, specify the entire range, such as 203.0.113.0/24.

> Warning: For security reasons, you shouldn't allow SSH access from all IPv4 addresses (0.0.0.0/0) to your instance, except for testing purposes and then only for a short time.


### What's going on?

Amazon Elastic Compute Cloud (Amazon EC2) provides scalable computing capacity in the Amazon Web Services (AWS) cloud. Using Amazon EC2 eliminates the need to invest in hardware up front. This enables applications to be developed and deployed faster. For example, only a few years ago to add a new database server meant getting various levels of approval from different departments, generating purchase orders and waiting for new hardware to arrive and be set-up. AWS means you can have a new instance of a database operational the same day you decide you need it. And quite possibly, before lunch!

EC2 is like a huge computer which runs either Windows or Linux. It can run whatever software or application you install on it. You could use EC2 to run  Apache, PHP, Python, databases, or other software. EC2 is an application hosting platform. You can setup the system to your requirements and run your application in the cloud.

EC2 uses EBS (Elastic Bean Stalk) Block Storage services. Your data is stored on a traditional block storage device — like a hard drive — over the network. AWS can provision a block storage device of any size and attach it to your virtual machine. From there, you would treat it like a normal disk. For example, you could format it with a filesystem and store files on it. Block storage is a familiar paradigm however one disadvantage is that you are allocating (and paying for) storage whether you use it or not. This is how data is stored on Amazon Machine Images (AMIs).

Amazon S3 (Simple Storage Service) is more a data storage solution/service. A great use of S3 is to make large binary files or other important data available in the cloud. Think of S3 system with a really big hard-disk that can hold any amount of data and be accessed anywhere in the world.

Amazon S3 uses object storage. This is the storage and retrieval of unstructured blobs of data and metadata using an HTTP API, or web service. Operating systems can't easily mount an object store like a normal disk. Object storage is useful for hosting static assets, saving user-generated content such as images, and storing backup files or logs.


### Questions
