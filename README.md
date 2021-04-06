# Serverless NLP Pipeline

This repository hosts a serverless NLP pipeline used to analyze sentiment. It consists of two lambda functions: producer and consumer. 

### Producer
- This lambda function will read from a DyanmoDB table and send values to SQS. A trigger will be need to be set up for this, I made it invoke every minute.

### Consumer 
- This lambda function will look up the contents of the message in SQS and will look up a corresponding wikipedia article. A trigger will also need to be set for this, I made it invoke everytime a message was delivered to SQS. The corresponding wikipedia is then passed to AWS Comprehend where sentiment analysis is performed. Results are then saved to a S3 bucket.

![Alt text](./flow.png?raw=true "Serverless Data Engineering Flow")

