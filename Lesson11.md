# Lesson 11 - Host a Java EE application on Elastic Beanstalk
<!-- http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/create_deploy_Java.html -->

You can deploy right from Eclipse. First you need to set up Eclipse to include the AWS Toolkit.

## Prerequisite: Install Eclipse

0. Make sure you have installed a version of Eclipse that is newer than Luna (Eclipse 4.4). So, Mars, Neon or Oxygen at the time of this writing. To download the latest version of Eclipse visit ```http://www.eclipse.org/downloads/```.

1. Install the AWS Toolkit in Eclipse. Open Help → Install New Software ...

2. Enter ```https://aws.amazon.com/eclipse``` in the text box labeled "Work with" at the top of the dialog.

3. Select the required "AWS Core Management Tools" and other optional items from the list. Select everything if you're not sure.

4. Click "Next" and let Eclipse guide you through the remaining installation steps.


## Create a new Access Key

Before you can use AWS with Eclipse you need to configure your Amazon account to use Eclipse. Eclipse will need an Access Key for your account. This will enable Eclipse to upload software you create to your instances.

> You can create or delete access keys for the root user on your AWS account. Anyone with the access key has unrestricted access to your account. They can do everything you can do.
The website for this feature is ```https://console.aws.amazon.com/iam/home?#/security_credential```

You can create or delete an access key any time. Once deleted, however, an access key can't be recovered or used.

1. To create, disable, or delete an access key for your AWS account root user, use your AWS account email address and password to sign in to the AWS Management Console. You are signing in as the AWS account root user.

2. Expand the Access keys (access key ID and secret access key) section.

3. Choose Create New Access Key. Then choose Download Key File to save the access key ID and secret access key to a file on your computer. After you close the dialog box, you can't retrieve this secret access key again. Ever.

## Create an AWS Java Web Project
1. After you’ve installed the AWS Toolkit for Eclipse, open the New AWS Java Web Project wizard.


## Deploy your Java web application to AWS Elastic Beanstalk

> You'll be using the AWS Toolkit for Eclipse.

1. Right/control click on this project in the Project Explorer

2. Choose run As, then Run on Server

3. Select the option to manually define a new server

4. Expand the Amazon Web Services folder, then select an appropriate version of Tomcat - 7.0 or newer

5. Select the AWS region where your application will be hosted

6. Provide a name for your application and environment and click Finish

7. Your project will be uploaded to AWS Elastic Beanstalk, which will provision infrastructure to host your application and deploy your code to it.

8. When it's finished, you will again see this page open up, now hosted in the cloud!



## What's going on?

AWS Elastic Beanstalk is the quickest and easiest way to deploy an AWS Java Web Project to the cloud.

AWS Elastic Beanstalk allows you to deploy web applications and web services developed with Java, .NET, PHP, Node.js, Python, Ruby, Go, and Docker. Elastic Beanstalk uses Apache, Nginx, Passenger (web app server that integrates with Apache and Nginx), and Microsoft Internet Information Server (IIS). Code uploaded to Elastic Beanstalk is automatically deployed.


Elastic Beanstalk works with the AWS Management Console, a Git repository, or an integrated development environment (IDE) such as Eclipse to upload your application. Elastic Beanstalk automatically handles most deployment details. Your application will be ready to use without any infrastructure or resource configuration efforts.

Elastic Beanstalk adjusts the required disk space and memory based on your application's specific needs. So as your application grows and requires more resources they are automatically added as needed.


## Questions

What is IAM? Identity and Access Management (IAM) allows you to control access to AWS services/resources. IAM, lets you create and manage AWS users and groups. It also allows you to set permissions on your AWS resources.


### Issues you may experience....

If you previously signed in to the console with IAM user credentials, your browser might remember this preference. In this case your account-specific sign-in page will be displayed and you cannot use the IAM user sign-in page.

If you see the IAM user sign-in page, choose Sign-in using root account credentials near the bottom of the page. This will send you back to the main sign-in page. From there, you can type your AWS account email address and password.


If you see a warning about accessing the security credentials for your AWS account, choose Continue to Security Credentials.
