# AWS S3 Lab
This lab is part of the [foundations trainings](https://github.com/octo-technology-downunder/octo-au-foundations) at [OCTO Technology Australia](http://careers.octo.com.au/).
In this lab we'll go though the following topics:
- Working with AWS Simple Storage Service (S3) via AWS Console and aws cli
- Creating/deleting buckets
- Working with files in buckets
- Managing Permissions, Versioning, Encryption
- Static website in S3 bucket

## Accessing S3 via AWS Web Console
1. Open https://console.aws.amazon.com/ and log in to your AWS account.
![Login](images/s3-login.png "Login")
1. Select Services -> S3 in drop down menu or search for S3 in service finder 
1. In opened page you'll see the list of buckets available in current AWS account

AWS web console provide intuitive and straightforward controls, you can easily manage buckets and objects inside buckets, performing create/read/update/delete operations 

NOTE that S3 is displayed as "global" service. To be precise, that means following things:
* Buckets are visible across all regions within AWS account
* Bucket names are unique across all AWS accounts worldwide
* Even though it's 'global', bucket still requires a region to be specified at creation time. That means objects in the bucket will be placed in multiple Availability zones of defined region


## Working with buckets and objects via aws cli
In order to access AWS services via CLI, awscli tool needs to be installed and configured. 
- Installation: follow the installation guide available here: https://docs.aws.amazon.com/cli/latest/userguide/installing.html
- Configuration: follow the configuration guide here: https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html

Once awscli is installed and configured, let's run some commands against aws S3:

* List all S3 buckets in AWS account:

    `aws s3 ls`
    
    Here we call `aws` tool with following format: `aws <service> <command>`
    Check that aws command output shows the same data as web console 
* Create new S3 bucket for our lab (replace <your_name> with your quadrigram):

    `aws s3 mb s3://temp-foundation-labs-s3-<your_name>`

    Here we created a bucket called `temp-foundation-labs-s3-<your_name>`
* Put a file to the bucket:

    `touch test_file.txt`<br>
    `echo "Hello AWS S3ca" > test_file.txt`<br>
    `aws s3 mv test_file.txt s3://temp-foundation-labs-s3-<your_name>`
    
    We just created a file `test_file.txt` and moved it to our S3 bucket 
* Let's see the objects in our bucket:
    
    `aws s3 ls temp-foundation-labs-s3-<your_name>`
    
    It should return the list of objects in our bucket, namely `test_file.txt` 
* We can also move objects inside s3:

    `aws s3 mv s3://temp-foundation-labs-s3-<your_name>/test_file.txt s3://temp-foundation-labs-s3-<your_name>/prefix/test_file.txt`
    
    Please note, that in fact this command has not created any objects in S3. It only updated _key_ of the object in S3. So, now our `test_file.txt` has _key_ equal to `prefix/test_file.txt` <br>
    At the same time, web console handles such kind of keys with `/` in it as hierarchy, making objects navigation more traditional

# Managing permission
