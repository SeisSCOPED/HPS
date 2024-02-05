# AWS S3
**Introduction to AWS S3, Buckets, Bucket Policies, and CLI Usage** 
Amazon Simple Storage Service (S3) is a scalable object storage service offered by Amazon Web Services (AWS). It allows users to store and retrieve any amount of data from anywhere on the web. S3 is designed for durability, availability, and scalability, making it a reliable solution for storing and managing data in the cloud.


## 1. What is AWS S3?

Amazon Simple Storage Service (S3) is a scalable object storage service offered by Amazon Web Services (AWS). It allows users to store and retrieve any amount of data from anywhere on the web. S3 is designed for durability, availability, and scalability, making it a reliable solution for storing and managing data in the cloud.

## 2. What is a Bucket in AWS S3?

In S3, a bucket is the fundamental container for storing objects (files or data). Each bucket has a globally unique name within the S3 namespace, and objects stored in a bucket can be organized into a hierarchy with a key (unique within a bucket). Buckets can be created, configured, and managed through the AWS Management Console, AWS SDKs (Software Development Kits), and AWS Command Line Interface (CLI).

## 3. What is a Bucket Policy?

A bucket policy in AWS S3 is a JSON-based configuration that defines rules for managing access to the objects within a bucket. It enables you to control permissions at the bucket level, allowing or denying access based on various conditions. Bucket policies can be used to grant or restrict access to specific AWS accounts, IP addresses, or even enforce encryption requirements. Properly configuring bucket policies is crucial for maintaining the security and privacy of your S3 data.

## 4. CLI Usage to Query S3

AWS provides a powerful command-line interface (CLI) that allows users to interact with S3 directly from the terminal. Here are some basic commands to get you started:

- **Install AWS CLI:**
  ```bash
  pip install awscli
  ```

- **Configure AWS CLI:**
    ```bash
    aws configure
    ```

    Follow the prompts to input your AWS Access Key ID, Secret Access Key, default region, and output format.

- **Test access with public bucket:**
    ```bash
    aws s3 ls s3://scedc-pds/
    ```

- **Create a Bucket:**
    ```bash
    aws s3 mb s3://your-bucket-name
    ```
    Buckets should use lowercase and dash.

- **List Buckets:**
    ```bash
    aws s3 ls s3://your-bucket-name
    ```
- **Upload a File to a Bucket:**
    ```bash
    aws s3 cp your-local-file.txt s3://your-bucket-name/
    ```

- **Download a File from a Bucket:**
    ```bash
    aws s3 cp s3://your-bucket-name/your-remote-file.txt your-local-destination/
    ```
    __Warning__ downloading from S3 comes with egress costs. We recommend only downloading small files and leaving big, reproducible files on S3 only for short term storage.



- **Bucket Policy:**
Policies on buckets are critical to allow roles (users and compute) to read-write on a bucket. For example, we want to create an S3 bucket policy that allows private writes (only authorized users can upload objects) and public reads (objects are publicly accessible), you can use the following example. This policy allows any AWS user to list the objects in the bucket and retrieve the objects, while restricting the ability to upload or modify objects to authorized users.

The following is an example of a bucket policy that allows public reads and authorized writes:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": "*",
            "Action": [
                "s3:GetObject"
            ],
            "Resource": "arn:aws:s3:::your-bucket-name/*"
        },
        {
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::YOUR_ACCOUNT_ID:root"
            },
            "Action": [
                "s3:PutObject",
                "s3:PutObjectAcl",
                "s3:DeleteObject"
            ],
            "Resource": "arn:aws:s3:::your-bucket-name/*"
        }
    ]
} 
```


Make sure to replace ```your-bucket-name``` with the actual name of your S3 bucket and ```YOUR_ACCOUNT_ID``` with your AWS account ID.

Explanation of the policy:

The first statement allows any user (``` "Principal": "*"```) to perform ```s3:GetObject``` actions on objects in the specified bucket.

The second statement allows specific actions (```"s3:PutObject"```,``` "s3:PutObjectAcl"```, ```"s3:DeleteObject"```) for the AWS root user of your account. This is to enable authorized users to upload, modify, and delete objects.

This policy ensures that public reads are allowed, while only authorized users (the account owner in this case) can perform write operations. Note that allowing public read access means that anyone with the object URL can access it, so use this configuration carefully and avoid placing sensitive information in the bucket.



## 5. Preparing an S3 bucket for Noisepy

Noisepy uses S3/Cloudstore to store the cross correlations and stacked data. For this step, it is important that your **user/role** and the **bucket** have the appropriate permissions for users to read/write into the bucket.

In the browser, please add the following policy to the bucket:


```json
{
    "Version": "2012-10-17",
    "Id": "Policy1674832359797",
    "Statement": [
            {
            "Sid": "Stmt1674832357905",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::<YOUR-ACCOUNT-ID>:role/<YOURROLE>"
            },
            "Action": "s3:*",
            "Resource": "arn:aws:s3:::<BUCKET-NAME>/*"
        }
    ]
}
```
In order to check whether the user can read/write in the bucket, we recommend testing from local:
```bash
aws s3 ls s3://<BUCKET-NAME>
```
Add a temporary file to make sure you have the credentials to add to the bucket
```bash
aws s3 cp temp s3://<BUCKET-NAME>
```

If this step works, and if your role and user account are attached to the bucket policy, the rest of the AWS noisepy tutorial should work.