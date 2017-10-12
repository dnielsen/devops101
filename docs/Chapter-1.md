# Chapter 1: AWS EC2 Sample Deployment

## Introduction
This document provides detailed, step-by-step instructions on creating a new AWS EC2 instance and deploying a provided Symfony2 application on it using LAMP (Linux, Apache, MySQL, PHP) stack. The application also uses a separate Redis database for certain parts of it, but the connection to Redis is not included in these instructions. These instructions are detailed for use with Mac OS; however, once logged onto the virtual machine provided by your EC2, the instructions are universal. The provided application is a website call “Spoutlet” and it will be cloned from an online github repository.

## Sign up for AWS Free Tier 
1. Sign up for Free Cloud Services – AWS Free Tier https://aws.amazon.com/free/
1. “AWS Free Tier includes offers that expire 12 months following sign up and others that never expire.”

## Create AWS EC2 instance
1. Log into AWS console and select “Services” > “EC2”
1. Click on “Instances”

## Launch Instance
1. Click on “Launch Instance”
1. Select “Ubuntu Server 16.04...”
1. Select the t2.medium size instance - VERY IMPORTANT
   1. click Next until you reach “Step 6: Configure Security Groups”
   1. Or Click on the “Step 6: Configure Security Groups” tab, located along the top of the screen.
1. Change “security group name” to “spoutlet - sample deployment”
1. Click “Add Rule”
   1. Add “Custom TCP Rule” with Port Range = 8000
   1. Change “Source” drop down menu to “Anywhere”
   1. Add “Custom TCP Rule” with Port Range = 0
   1. Change “Source” drop down menu to “Anywhere”
1. Click “Add Rule”
   1. Add “HTTP”
   1. Change “Source” drop down menu to “Anywhere”
1. Click “Review and Launch”, then review settings and click “Launch”
1. A pop up window will appear asking about a security key
   1. Select “Create a new key pair” from the first drop down box
   1. Enter “spoutlet_sampleEC2” for the key pair name, then “Download Key Pair”
   ***IMPORTANT*: after downloading the key, place it in a safe and accessible place (perhaps create a backup copy and store it in the cloud); if lost, your key cannot be replaced, as AWS does not store your key after its creation
1. After storing your key in a safe and secure location, click “Launch Instances”



...

https://docs.google.com/document/d/1dKerau5K38A8iIsPBdc2c8Eg9uXMaTRLIUDICKKuOSY/edit - automatic!
[DevOps](http://http://bit.ly/2fTJxQ8)


