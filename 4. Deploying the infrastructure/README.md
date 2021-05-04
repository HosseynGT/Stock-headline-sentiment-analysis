# Deploying the infrastructure

Unfortunately, I can not get the infrastructure but I have some experience with GCP MLEngine.

How will you host the API in the cloud?

- In GCP MLEngine you can define NGINX endpoint enabled that can handle traffic balancing 

What other services does the API need to interact with? 

- YFinance for gathering stocks history
- TFhub for getting pretrain Bert model only for training

How will you update the model continuously as more data becomes available? 

- with TFRecord files, the apache beam can continuously read and write new samples into training data.

Are there any trade-offs to the architecture that you are choosing (serverless is definitely not always the right answer)? 

- It is hard to write unit tests for serverless architecture. For stream data pipelines, because of the event-based nature of 	serverless architecture we face many problems, a microservice architecture can help a lot.

How do you perform successful monitoring and logging to catch errors?

- GCP MLEngine has good logging and monitoring tools, but as I said in the previous chapter, we can integrate our container with Prometheus for monitoring resources, API calls, and...

