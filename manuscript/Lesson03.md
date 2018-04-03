# Lesson 03 - Store Files on an S3 Bucket

Buckets are globally unique containers for everything that you store in Amazon S3.	After you create a bucket, you can upload your objects (for example, your photo or video files).	By default, the permissions on an object are private, but you can set up access control policies to grant permissions to others.

## Learning Objectives
* Create a new bucket
* Upload your data
* Set up your permissions

## Walkthrough

Before you can upload data to Amazon S3, you must create a bucket in one of the AWS Regions to store your data in. After you create a bucket, you can upload files to your bucket.

1. Sign in to the AWS Management Console and open the Amazon S3 console at ```https://console.aws.amazon.com/s3/```

2. If you copied settings from another bucket, choose Create. Most likely, you did not, so choose [Next].

3. Keep choosing [Next] to accept all the defaults until you get to the end when you see the [Create bucket]

4. Click the [Create bucket] button

5. You will now see your bucket.

6. If you select your bucket you can upload files.

7. Go back to the bucket. Click on a file. A PDF or image would be best for this lesson. Copy the link for the file to another browser tab. It shouldn't work because it isn't public.

8. Go back to the bucket. Click on the file again. Select the option to make public. Now you can view the link to the file in another tab or even on another computer. You can try emailing it to your phone and accessing it that way.

9. Go back to the bucket. Delete the file. The bucket still exists.

10. Now, delete the entire bucket. Note - you didn't have to delete the files first before deleting the bucket.

## What's going on

Amazon S3 (Simple Storage Service) makes files available anywhere in the world. You can set policies on the files or on the buckets or both.

## Questions
