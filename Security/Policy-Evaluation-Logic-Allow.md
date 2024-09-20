Lab Description

The goals of this task are to explore the process of evaluating policies and to configure both an identity-based policy and a resource-based policy for a specific role.
Task Resources

Region-specific resources are created in the us-east-1 region. For more details about regional services, see AWS Services by Region.

In this task, you will work with the following resources:

    IAM Role cmtr-4960e3c6-iam-pela-iam_role: You will grant specific permissions to this role and verify that they are applied successfully.
    S3 Bucket cmtr-4960e3c6-iam-pela-bucket-1-9017607: A bucket with a default configuration and one object inside. A resource-based S3 bucket policy should be created for this bucket.
    S3 Bucket cmtr-4960e3c6-iam-pela-bucket-2-9017607: An empty bucket used solely for task verification purposes. Do not attach any policies to this bucket or change its configuration.

Objectives

In two moves, you must:

    Create and attach an identity-based policy to the cmtr-4960e3c6-iam-pela-iam_role role that allows all buckets to be listed.
    Create a resource-based S3 bucket policy that allows to get and put an object as well as list the objects in the cmtr-4960e3c6-iam-pela-bucket-1-9017607 bucket. The cmtr-4960e3c6-iam-pela-iam_role role must be allowed to perform all of these actions for the cmtr-4960e3c6-iam-pela-bucket-1-9017607 bucket only; do not allow access to all principals.

One move is to create, update, or delete an AWS resource. Some verification steps may pass without any action, but to complete the task, you must ensure that all the steps are passed.
Task Verification

To make sure everything has been done correctly, use the AWS policy simulator for the cmtr-4960e3c6-iam-pela-iam_role role and check that:

    You can list all the buckets.
    You can list, get, and put objects only in the cmtr-4960e3c6-iam-pela-bucket-1-9017607 bucket.
    You can't list, get, or put objects in the cmtr-4960e3c6-iam-pela-bucket-2-9017607 bucket.

Optionally: Instead of using the AWS policy simulator, you can assume the role and perform the required checks.

========
Solution: 
1. Create and Attach an Identity-Based Policy to the IAM Role

This policy will allow the IAM role (cmtr-4960e3c6-iam-pela-iam_role) to list all S3 buckets.
Steps:

    Go to the IAM console.
    In the left navigation pane, click on Roles.
    Find and select the role cmtr-4960e3c6-iam-pela-iam_role.
    Under the Permissions tab, click Add permissions.
    Select Create inline policy.
    Choose the Service as S3.
    Under Actions, choose ListAllMyBuckets (this allows listing all S3 buckets).
    Under Resources, choose All resources (this is because you want to list all buckets, not just specific ones).
    Click Review policy.
    Give the policy a name like ListAllBucketsPolicy and click Create policy.

This policy allows the role to list all the S3 buckets.

2. Create a Resource-Based Policy for the S3 Bucket

Next, you need to create a resource-based policy for the specific bucket (cmtr-4960e3c6-iam-pela-bucket-1-9017607) to allow the IAM role to get, put, and list objects.
Steps:

    Go to the S3 console.
    In the left navigation pane, click on Buckets.
    Find and select the bucket cmtr-4960e3c6-iam-pela-bucket-1-9017607.
    Under the Permissions tab, scroll down to Bucket policy.
    Click Edit to add a new bucket policy.

Here is an example policy to allow cmtr-4960e3c6-iam-pela-iam_role to list, get, and put objects in the bucket:


{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::your-account-id:role/cmtr-4960e3c6-iam-pela-iam_role"
      },
      "Action": [
        "s3:ListBucket"
      ],
      "Resource": "arn:aws:s3:::cmtr-4960e3c6-iam-pela-bucket-1-9017607"
    },
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::your-account-id:role/cmtr-4960e3c6-iam-pela-iam_role"
      },
      "Action": [
        "s3:GetObject",
        "s3:PutObject"
      ],
      "Resource": "arn:aws:s3:::cmtr-4960e3c6-iam-pela-bucket-1-9017607/*"
    }
  ]
}

Replace your-account-id with your AWS account ID.
This policy grants the cmtr-4960e3c6-iam-pela-iam_role permission to list, get, and put objects in cmtr-4960e3c6-iam-pela-bucket-1-9017607 but no access to any other buckets.
Click Save changes.


```terraform
terraform {
  required_providers {
    routeros = {
      source = "terraform-routeros/routeros"
    }
  }
}

provider "routeros" {
  hosturl  = "(http|https|api|apis)://my.router.local[:port]"
  username = "my_username"
  password = "my_super_secret_password"
}

```