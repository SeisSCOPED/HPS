# AWS Lambda

```{warning}
This page may not be updated. For the latest HPS book, please visit https://seisscoped.org/HPS-book
```

AWS Lambda is a serverless computing service provided by Amazon Web Services (AWS). It allows to run code without provisioning or managing servers. Instead, Lambda automatically scales the application by running code in response to triggers and events.

Here's how it works:

* **Event Sources**: Lambda functions are triggered by various events such as changes to data in an S3 bucket, updates to DynamoDB tables, API Gateway requests, SNS notifications, etc.  In seismology, this could be like data streaming to S3, or daily updates of a seismic network on S3.

* **Function Execution**: When an event occurs, Lambda executes the function by spinning up a compute environment, running the code, and then shutting down the environment when the function execution is complete. This could be  event detection, picking, classification, or it could be low-computation of feature extraction in continuous time series 

* **Scaling**: Lambda automatically scales to handle a large number of requests. You don't need to worry about provisioning or managing servers; AWS takes care of this for you.

Lambda supports several programming languages, including Python, Node.js, Java, C#, Go, and Ruby.


