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
## Solution
```bash
aws sqs receive-message --queue-url https://sqs.us-east-1.amazonaws.com/092297851374/wiz-tbic-analytics-sqs-queue-ca7a1b2 --attribute-names All --message-attribute-names All --max-number-of-messages 10
```
```json
        {
            "MessageId": "4283849c-74d4-4825-9227-5da464b23f0b",
            "ReceiptHandle": "AQEBgnKZ95I3BiEISAe/De63+KS/yADoAemJ8e3a8ZHeqaWaUTTJaceNsnwf2vqFUOU/tJCVR7xtdj+CgUXQazQKuUzBKlAegWNfQJecGJB8A4qnFIYOpKPRfBCkQ8Fah7JINcisCKsl01D0sTcMLam0VREtifPFsr6GmgNoW0d/sLdYI5gIKoHgp8qJzhSSocrWkC2moQH18bVihmCe0qMSYZ2rBHYnAUjsCxNHBFCC1UjpZYDTUK+Z74sn8j6mQuAnq+5mpHvHog9BrRqrhzSEl2MzdrF5ETG6Fi9MXDoyONlHNsPaFceUjM1Hyx8M+FAm78Sv3MFn6+aRmv/RBqvKuU8iKAZHCAtNBOru/Kex6RM+VbzCprocSnGZe3Ys6KCjnw7FUhJmSLi/1/v3jvxDbG8QWrR/CUUM+a+AYQ9VLa8=",
            "MD5OfBody": "4cb94e2bb71dbd5de6372f7eaea5c3fd",
            "Body": "{\"URL\": \"https://tbic-wiz-analytics-bucket-b44867f.s3.amazonaws.com/pAXCWLa6ql.html\", \"User-Agent\": \"Lynx/2.5329.3258dev.35046 libwww-FM/2.14 SSL-MM/1.4.3714\", \"IsAdmin\": true}",
            "Attributes": {
                "SenderId": "AROARK7LBOHXGHGQ5XCT5:tbic-wiz-send-flag-to-sqs-8d265a4",
                "ApproximateFirstReceiveTimestamp": "1691583093512",
                "ApproximateReceiveCount": "1",
                "SentTimestamp": "1691582488541",
                "AWSTraceHeader": "Root=1-64d38018-6a56d23a731e0cbe6ed85687;Parent=1e4dbd311c45e826;Sampled=0;Lineage=037bed70:0"
            }
        }
```

```bash
 curl https://tbic-wiz-analytics-bucket-b44867f.s3.amazonaws.com/pAXCWLa6ql.html
{wiz:you-are-at-the-front-xxxxxxxxxxx}
```
