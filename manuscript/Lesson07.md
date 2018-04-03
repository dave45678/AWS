# Lesson 07 - Host an Angular Application
<!--https://medium.com/codefactory/angular2-s3-love-deploy-to-cloud-in-6-steps-3f312647a659-->

## Learning Objectives

## The Walkthrough

### Enable Website Hosting on S3
1. Sign in to the AWS Management Console and open the Amazon S3 console at ```https://console.aws.amazon.com/s3/```

2. Create a bucket.
	* Click the Create Bucket Button
	* On the Name and Region page, type a name for your bucket and choose the AWS Region where you want the bucket to reside. Complete the fields on this page as follows:

		* For Bucket name, type a unique DNS-compliant name for your new bucket. Follow these naming guidelines:

			* The name must be unique across all existing bucket names in Amazon S3.
			* The name must not contain uppercase characters.
			* The name must start with a lowercase letter or number.
			* The name must be between 3 and 63 characters long.
			* After you create the bucket you cannot change the name, so choose wisely.
			* Choose a bucket name that reflects the objects in the bucket because the bucket name is visible in the URL that points to the objects that you're going to put in your bucket.

		* For Region, choose the AWS Region where you want the bucket to reside. Choose a Region close to you to minimize latency and costs, or to address regulatory requirements. Objects stored in a Region never leave that Region unless you explicitly transfer them to another Region. For a list of Amazon S3 AWS Regions, see Regions and Endpoints in the Amazon Web Services General Reference.

	* Click Next, and on the Set properties page, you can configure the following properties for the bucket. Or, you can configure these properties later, after you create the bucket.

		* Versioning – Versioning enables you to keep multiple versions of an object in one bucket. Versioning is disabled for a new bucket by default. For information on enabling versioning, see How Do I Enable or Suspend Versioning for an S3 Bucket?.

		* Server access logging – Server access logging provides detailed records for the requests that are made to your bucket. By default, Amazon S3 does not collect server access logs. For information about enabling server access logging, see How Do I Enable Server Access Logging for an S3 Bucket?.

		* Tags – With AWS cost allocation, you can use tags to annotate billing for your use of a bucket. A tag is a key-value pair that represents a label that you assign to a bucket. To add tags, choose Tags, and then choose Add tag. For more information, see Using Cost Allocation Tags for S3 Buckets in the Amazon Simple Storage Service Developer Guide.

		* Object-level logging – Object-level logging records object-level API activity by using CloudTrail data events. For information about enabling object-level logging, see How Do I Enable Object-Level Logging for an S3 Bucket with AWS CloudTrail Data Events?.

		* Default encryption – Amazon S3 default encryption provides a way to set the default encryption behavior for an S3 bucket. You can set default encryption on a bucket so that all objects are encrypted when they are stored in the bucket. For information about enabling default encryption, see How Do I Enable Default Encryption for an S3 Bucket?.

	* Click Next, on the Set permissions page, you manage the permissions that are set on the bucket that you are creating. You can grant read access to your bucket to the general public (everyone in the world). Granting public read access is applicable to a small subset of use cases such as when buckets are used for websites. We recommend that you do not change the default setting of Do not grant public read access to this bucket. You can change permissions after you create the bucket.

	* Click Next, on the Review page, verify the settings. If you want to change something, choose Edit. If your current settings are correct, choose Create bucket.

3. Open the bucket Properties pane, choose Static Website Hosting, and do the following:
	* Choose Enable website hosting.
	* In the Index Document box, type the name of your index document. The name is typically index.html.
	* Choose Save to save the website configuration.
	* Write down the Endpoint. This is the Amazon S3-provided website endpoint for your bucket. You use this endpoint in the following steps to test your website.

4. Add Bucket Policy
	* In the Properties pane for the bucket, choose Permissions.
	* Choose Add Bucket Policy.
	* Copy the following bucket policy, and then paste it in the Bucket Policy Editor.

```json
{
   "Version":"2012-10-17",
   "Statement":[{
 	"Sid":"PublicReadForGetBucketObjects",
         "Effect":"Allow",
 	  "Principal": "*",
       "Action":["s3:GetObject"],
       "Resource":["arn:aws:s3:::example-bucket/*"
       ]
     }
   ]
 }
```
	* In the policy, replace example-bucket with the name of your bucket.
	* Choose Save.

### Deploy the Angular Application

1. From the terminal in your Angular project directory run the following command: ```ng build --prod```.  This creates a new folder in your Angular application called *dist*. The files in the dist folder need to be uploaded to your S3 website.


2. Upload the contents of ```dist``` folder to the root folder of the S3 website. Do not upload the *dist* folder.


3. Testing Your Application
	* Type the following URL in the browser, replacing example-bucket with the name of your bucket and website-region with the name of the AWS Region where you deployed your bucket.

```
http://example-bucket.s3-website-region.amazonaws.com
```

If your browser displays your index.html page, the website was successfully deployed.


## What's Going On

#### Configuring Index Document Support
An index document is a webpage that Amazon S3 returns when a request is made to the website. If a user enters http://www.mysite.com in the browser, the user is not requesting any specific page. In that case, Amazon S3 returns the index document also known as the default page.
When you configure your bucket as a website, provide the name of the index document. You then upload a file with this name and set it to be publicly readable.


## Questions

Bonus: create a web service on another instance and add a link to each to point to the other.
