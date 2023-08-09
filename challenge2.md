# Challenge 2

## We created our own analytics system specifically for this challenge. We think it's so good that we even used it on this page. What could go wrong?
- https://bigiamchallenge.com/challenge/2

## IAM Policy
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": "*",
            "Action": [
                "sqs:SendMessage",
                "sqs:ReceiveMessage"
            ],
            "Resource": "arn:aws:sqs:us-east-1:092297851374:wiz-tbic-analytics-sqs-queue-ca7a1b2"
        }
    ]
}
```
## View Source
```javascript

// Initialize the Amazon Cognito credentials provider
  AWS.config.region = 'us-east-1';
AWS.config.credentials = new AWS.CognitoIdentityCredentials({IdentityPoolId: 'us-east-1:c6f3eb2e-3cb5-404e-93bc-f0bdf7ad042e'});
// Set the region
AWS.config.update({region: 'us-east-1'});

// Create an SQS service object for Web Analytics.
// Log trafic from all users into SQS.
var sqs = new AWS.SQS({apiVersion: '2012-11-05'});

var params = {
  DelaySeconds: 0,
  MessageBody: JSON.stringify({"URL": document.location.href, "User-Agent": navigator.userAgent, "IsAdmin": false}),
  QueueUrl: "https://sqs.us-east-1.amazonaws.com/092297851374/wiz-tbic-analytics-sqs-queue-ca7a1b2"
};

sqs.sendMessage(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data.MessageId);
  }
});

```
