<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## SNS

What if you want to send one message to many receivers?
![img.png](../images/sns_pub_sub.png)
Pub / Sub = publish subscribe  
Publishing a message into a topic  
Subscribing from that topic

* The “event producer” only sends message to one SNS topic
* As many “event receivers” (subscriptions) as we want to listen to the SNS topic notifications
* Each subscriber to the topic will get all the messages (note: new feature to filter messages)
* Up to 12,500,000 subscriptions per topic
* 100,000 topics limit
* Protocols for SNS: Amazon Kinesis Date Firehose, Amazon SQS, Amazon Lambda, Email, Email-JSON, HTTP, HTTPS, SMS. But not for: Kinesis Data Streams!
  ![img.png](../images/sns_publish_subscribers.png)

### SNS integrates with a lot of AWS services
* Many AWS services can send data directly to SNS for notifications
  ![img.png](../images/sns_integrations_aws_services.png)

### Amazon SNS – How to publish
* Topic Publish (using the SDK)
    - Create a topic
    - Create a subscription (or many)
    - Publish to the topic
* Direct Publish (for mobile apps SDK)
    - Create a platform application
    - Create a platform endpoint
    - Publish to the platform endpoint
    - Works with Google GCM, Apple APNS, Amazon ADM…

### Amazon SNS – Security
* Encryption:
    - In-flight encryption using HTTPS API
    - At-rest encryption using KMS keys
    - Client-side encryption if the client wants to perform encryption/decryption itself
* Access Controls: IAM policies to regulate access to the SNS API
* SNS Access Policies (similar to S3 bucket policies)
    - Useful for cross-account access to SNS topics
    - Useful for allowing other services ( S3…) to write to an SNS topic

### SNS + SQS: Fan Out
* Push once in SNS, receive in all SQS queues that are subscribers
* Fully decoupled, no data loss
* SQS allows for: data persistence, delayed processing and retries of work
* Ability to add more SQS subscribers over time
* Make sure your SQS queue access policy allows for SNS to write
* Cross-Region Delivery: works with SQS Queues in other regions
  ![img.png](../images/sns_sqs_fan_out.png)

### Application: S3 Events to multiple queues
* For the same combination of: event type (e.g. object create) and prefix (e.g. images/) you can only have one S3 Event rule
* If you want to send the same S3 event to many SQS queues, use fan-out
  ![img.png](../images/s3_sns_sqs_multiple_queues.png)

### Application: SNS to Amazon S3 through Kinesis Data Firehose
* SNS can send to Kinesis and therefore we can have the following solutions architecture:
  ![img.png](../images/sns_s3_kinesis_firehose.png)

### Amazon SNS – FIFO Topic
* FIFO = First In First Out (ordering of messages in the topic)
  ![img.png](../images/sns_fifo_topic.png)
* Similar features as SQS FIFO:
    - Ordering by Message Group ID (all messages in the same group are ordered)
    - Deduplication using a Deduplication ID or Content Based Deduplication
* Can have SQS Standard and FIFO queues as subscribers
* Limited throughput (same throughput as SQS FIFO)

### SNS FIFO + SQS FIFO: Fan Out
* In case you need fan out + ordering + deduplication
  ![img.png](../images/sns_fifo_sqs_fifo_fan_out.png)

### SNS – Message Filtering
* JSON policy used to filter messages sent to SNS topic’s subscriptions
* If a subscription doesn’t have a filter policy, it receives every message
  ![img.png](../images/sns_message_filtering.png)