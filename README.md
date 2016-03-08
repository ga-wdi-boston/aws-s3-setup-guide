![General Assembly Logo](http://i.imgur.com/ke8USTq.png)

# Simple Storage Service on Amazon Web Services setup

## Instructions

Follow the steps outlined to create and gain access to an AWS S3 bucket.

## Prerequisites

-   An `AWS` _(Amazon Web Services)_ account

## AWS access control

We'll go through the steps necessary to allow authenticated uploads without
 allowing other access to AWS.

From the `AWS` console open tabs for `IAM` _(Identity and Access Management)_
 and `S3` _(Simple Storage Service)_.

In the IAM tab, select `Users` and then the IAM user you want to use for
 uploads.  Alternatively, you can create and select a new user.  We'll need the
 `User ARN` _(Amazon Resource Name)_ to grant access to the S3 bucket we'll use
 for uploads.  We'll also need an `Access Key` _(Access Key Id and Secret Access
 Key)_ for this IAM User to upload files.

In the S3 tab, create a new bucket for uploads.  Open `Permissions` and click on
 `Add bucket policy`.  Click on `AWS Policy Generator` at the bottom of the
 `Bucket Policy Editor` modal.  This will open the AWS Policy Generator page.

On the AWS Policy Generator page, select `S3 Bucket Policy` as the type of
 policy to generate.
Copy the User ARN from the IAM user page and paste it into the `Principal` text
 box.
Select `Amazon S3` as the `AWS Service`.
Select `PutObject` and `PutObjectAcl` in the actions multi-select.
Enter `arn:aws:s3:::<bucket_name>/<key_name>` into the `Amazon Resource Name
 (ARN)` text box.
`key_name` is a directory equivalent, we'll use `*`.
After all that, click the `Add Statement` button then the `Generate Policy`
 button.
The `Policy JSON Document` modal that opens contains the bucket policy we'll use
 (an example follows).
Select and copy the JSON from the modal then go back to the S3 tab and paste the
 JSON into the Bucket Policy Editor and click save.

```json
{
  "Version": "2012-10-17",
  "Id": "Policy1439826519004",
  "Statement": [
    {
      "Sid": "Stmt1439826516658",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::<AWS Account Id>:user/<IAM User Name>"
      },
      "Action": [
        "s3:PutObjectAcl",
        "s3:PutObject"
      ],
      "Resource": "arn:aws:s3:::<bucket_name>/<key_name>"
    }
  ]
}
```

With this configuration, we only allow upload access to this the one bucket.

This is one specific and restrictive way of implementing access control.
AWS provides many different mechanisms to grant and restrict access.

## [License](LICENSE)

Source code distributed under the MIT license. Text and other assets copyright
General Assembly, Inc., all rights reserved.
