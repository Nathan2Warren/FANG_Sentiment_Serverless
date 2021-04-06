# Serverless NLP Pipeline

This repository hosts a serverless NLP pipeline used to analyze sentiment. It consists of two lambda functions: producer and consumer. 

### Producer
- This lambda function will read from a DyanmoDB table and send values to SQS. A trigger will be need to be set up for this, I made it invoke every minute.

### Consumer 
- This lambda function will look up the contents of the message in SQS and will look up a corresponding wikipedia article. A trigger will also need to be set for this, I made it invoke everytime a message was delivered to SQS. The corresponding wikipedia is then passed to AWS Comprehend where sentiment analysis is performed. Results are then saved to a S3 bucket.

### Serverless Pipeline Flow
![Alt text](./flow.png?raw=true "Serverless Data Engineering Flow")

### How to use:
1. Set up your DynamoDB table, S3 bucket, SQS queue, and IAM roles to start. Doing this at the start makes potential debugging much easier. 


2. In Cloud9, locate the aws-explorer tab (on the left). Go to lambda, right click and select "Create SAM lambda application." You will need to specify the workspace, python version (I used 3.7), and template (hello-world). 


3. Modify `app.py` to include the code from the `app.py` files in my producer folder. Edit the code to ensure that your DynamoDB table and SQS queue names match.


4. Deploy the SAM lambda application using the following code:
```
sam build --use-container
sam deploy--guided
```
5. In AWS Lambda you should now see your producer app. Click the trigger and then click permissions. If you have an IAM role set up, assign it, otherwise you may add policies so that the producer lambda function will be able to run. 

6. Delete the original trigger and set up one using 'EventBridge.' You will want to create a new rule and write the "schedule expression". I used: rate(1 minute). 

7. When enabled, messages should populate in your SQS queue. You can disable this trigger until consumer is complete so that SQS stops populating.

8. For the consumer, repeat steps 2-5 using the `app.py` file located in the consumer directory. 

9. Delete the original trigger and create a SQS trigger that will pull from your SQS queue. 

10. Check CloudWatch logs to check for errors and your S3 bucket to view the output of the sentiment analysis performed. 

11. Shutdown triggers when finished.
