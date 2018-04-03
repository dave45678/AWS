# Lesson 02 - Host a Static Web Page

## Prequisites

Be sure you have completed the following tasks from Lesson 0
* AWS CLI - need to install the command line interface to your local computer
* S3 - simple storage service
* IAM - identity and access management
## Learning Objectives

* Learn about S3 and uploading files
* Use CloudFront addresse geographic latency issues
* Create a bucket to store files and set permissions

## The Walkthrough

1. Learn about S3
* Amazon S3 (Simple Storage Service) is a web service
* S3 uses the same scalable storage infrastructure that Amazon.com uses to run its own global e-commerce network.
* The maximum file size of any object you can store in S3 is 5 terrabytes
* Objects are stored in buckets. A bucket is a collection of files and metadata about the file
* You can set permissions on buckets and object
* Bucket names must be unique across all the world

Because bucket names and objects are accessible via HTTP, they are perfect for hosting static websites

### Configure permissions for your bucket
1. create the bucket
2. use the AWS policy generator to create a JSON formatted policy for your bucket
3. select the following settings:
1.
```cmd
Effect = Allow,
Principal = *
AWS Service = Amazon S3
ARN = your ARN for your bucket (Amazon Resource Name)
```

4. Go back to your bucket and paste the policy in the policy textbox

```json
{
  "Version":"2012-10-17",
  "Statement":[{
	"Sid":"PublicReadGetObject",
        "Effect":"Allow",
	  "Principal": "*",
      "Action":["s3:GetObject"],
      "Resource":["arn:aws:s3:::example-bucket/*"
      ]
    }
  ]
}
```

5. you can upload objects using the console or the CLI

### To enable website hosting for an Amazon S3 bucket

1. Sign in to the AWS Management Console and open the Amazon S3 console at https://console.aws.amazon.com/s3/.
2. In the list, choose the bucket that you want to use for your hosted website.
3. Choose the Properties tab.
4. Choose Static website hosting, and then choose Use this bucket to host a website.
5. You are prompted to provide the index document and any optional error documents and redirection rules that are needed.

To host a static website, you first configure an Amazon S3 bucket for website hosting (above), and then upload your website content to the bucket. The website is then available at the AWS Region-specific website endpoint of the bucket, which is in one of the following formats:

```cmd
<bucket-name>.s3-website-<AWS-region>.amazonaws.com
 - or -
<bucket-name>.s3-website.<AWS-region>.amazonaws.com
```
### Configure Index Document Support

An index document is a webpage that Amazon S3 returns when a request is made to the root of a website or any subfolder. For example, if a user enters http://www.example.com in the browser, the user is not requesting any specific page. In that case, Amazon S3 serves up the index document, which is sometimes referred to as the default page.

When you configure your bucket as a website, provide the name of the index document. You then upload an object with this name and configure it to be publicly readable.

The trailing slash at the root-level URL is optional. For example, if you configure your website with index.html as the index document, either of the following two URLs return index.html.

http://example-bucket.s3-website-region.amazonaws.com/
http://example-bucket.s3-website-region.amazonaws.com

### Upload files with the CLI
1. install the CLI including your Access key etc…
From the command prompt type the following:
```cmd
cp
```
2. Open the s3 console and click on your bucket
3. View the link from the object list



## What you should see

#### This URL returns a default index document that you configured for the website.

```http://examplebucket.s3-website-us-west-2.amazonaws.com/```

#### This URL requests the photo.jpg object, which is stored at the root level in the bucket.

```http://examplebucket.s3-website-us-east-1.amazonaws.com/photo.jpg```

#### This URL requests the docs/doc1.html object in your bucket.

```http://examplebucket.s3-website-us-east-1.amazonaws.com/docs/doc1.html```

## What's Going On

## Questions
