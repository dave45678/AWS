# Lesson 01 - Create an AMI (Amazon Machine Instance) with Lightsail

Released in 2016, Lightsail is a flat-rate solution with easy setup and low maintenance. Amazon Lightsail is a scaled-down version of Elastic Cloud (EC2).

Setting up a Lightsail VPS is fairly straightforward. The Lightsail website can be accessed from the AWS console.

You can create a Linux/Unix-based Lightsail instance (a virtual private server) running an application like WordPress or a development stack like LAMP in seconds. After your instance starts running, you can connect to it via SSH without leaving Lightsail.

<!--https://cloudacademy.com/blog/how-to-set-up-your-first-amazon-lightsail/-->


## Learning Objectives

* Create a virtual machine with Lightsail
* Create an Ubuntu with the MEAN stack installed
* Explore Amazon Linux

## The Walkthrough

1. Log in to your console

2. Browse to *https://lightsail.aws.amazon.com/ls/webapp/home/instances*

3. Click on the orange button to create an instance (virtual machine). When you create an instance using Lightsail you choose an operating system (OS). You can also choose an instance image that has software installed. In our case we're going to select the image with the MEAN stack so we can create Angular applications.

4. Accept the default (selected) instance location

5. You will need to choose a platform. Select Linux. Then Select the MEAN Stack so you can upload Angular applications.

## What's Going On



## Questions

* What is MEAN?
MEAN is a collection of JavaScript-based technologies. The MEAN stack consists of MongoDB, Express.js, Angular, and Node.js — used to develop web applications.

* What is Express.js?
Express.js is a web application framework to help organize your web application into an MVC architecture on the server side. So it works with Node.js and MongoDB. Angular works in the browser on the client side.

* What is SSH?
SSH (Secure Socket Shell) is a network protocol that provides a secure connection to a remote computer.

* What is my user name and password?
Your username is *bitnami*
You can find out your password by typing the following command:
````cat bitnami_application_password```


* What is Bitnami?
Bitnami is a library of installers or software packages for web applications and development stacks as well as virtual appliances.
