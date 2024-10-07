# High-Level Design (HLD) for Lift-and-Shift Migration to AWS

# Introduction
This document presents the high level design for migrating the API based application deployed on two virtual machine’s WebServerVM and SQLVM to the AWS cloud environment. The migration will employ AWS Lambda for serverless architecture with Amazon RDS with Multi-AZ replication for the server. The purpose of doing this is to enhance high availability, scalability and the importance of minimizing the down time during the time of transfer and the maximum down time allowed will be six hours.

# Target Architecture Components

**AWS Lambda**: The API Application WebServerVM will be turned into AWS Lambda functions. This eliminates the need to worry about infrastructure maintenance as AWS Lambda is designed for serverless compute which will scale up or down as the demand BECOMES MORE OR LESS without manual scaling and high availability guarantee.

**Amazon API Gateway**: The API Gateway is an interface residing between clients and Lambda functions, receiving all HTTP requests to the Lambda functions. Additionally, it will forward requests to the suitable Lambda function or resources and snip a bid on security and monitoring.

**Amazon RDS (MySQL)**: The MySQL database located in SQLVM would be migrated to Amazon RDS (Multi-AZ). This ensures that there is a standby in another Availability Zone that database records are stand by ready to be acted upon in the event that one goes down.

**Amazon CloudWatch & AWS X-Ray**: CloudWatch will be tasked with tracking performance metrics, generating insights and notifications related to the serverless environment and performance of RDS. No, the AWS X-Ray will be specifically for API application debugging and tracing.

**Amazon S3**: Also S3 will be used to store any static assets concerning the API application as such items are highly durable and capable of high storage expansion.

**Amazon Route 53**: Route 53 will carry out the DNS traffic management ensuring low response times to queries made to the API Gateway while still allowing for optimal recovery in the event of a requesting failure if necessary.

## Target Architecture Diagram

                                ┌────────────────────┐
                                │    Amazon Route 53 │
                                │(DNS with Failover) │
                                └────────────────────┘
                                          │
                                          ▼
                                ┌────────────────────┐
                                │    API Gateway     │
                                │(Entry point for    │
                                │ client requests)   │
                                └────────────────────┘
                                          │
                                          ▼
                                ┌────────────────────┐
                                │    AWS Lambda      │
                                │(Compute Layer for  │
                                │ API Logic)         │
                                └────────────────────┘
                                          │
                                          ▼
                                ┌────────────────────┐
                                │   Amazon RDS       │
                                │ MySQL (Multi-AZ)   │
                                │(Primary DB in AZ1, │
                                │ Standby in AZ2)    │
                                └────────────────────┘
                                          │
                                          ▼
                                ┌────────────────────┐
                                │  Amazon CloudWatch │
                                │(Monitoring & Alert)│
                                └────────────────────┘
                                          │
                                          ▼
                                ┌────────────────────┐
                                │    Amazon S3       │
                                │ (Optional: Static  │
                                │ Content Storage)   │
                                └────────────────────┘



## Advantages of the Target Architecture

**High Availability**: Due to AWS Lambda and the multi-AZ replications of the Amazon RDS the application is fault tolerant. For example, if an issue occurs in one of the availability zones, resources will be directed without manual efforts to optimizing healthy resources.

**Scalability**: Regardless of how many requests are coming, the service will scale automatically eliminating worries of provisioning above or lower than what is required.

**Cost Efficiency**: There is no money wasted on idle EC2 instances in AWS Lambda as, you only pay for the amount of compute power which is utilized by the API requests.

**Reduced Downtime**: Serverless computing combined with the RDS + Multi-AZ provides failover architecture that minimizes unplanned outages to unreasonable periods expected to be less than 6 hours.


# Migration Steps

**Step 1: Refactor Web Application to Serverless**

**Assessment and Planning**: For existing API based application, try to identify few components which can be implemented in AWS Lambda. Prepare documentation on the API endpoints and write up the code for the Lambda function.

**Develop Lambda functions**: Breakdown the business logic which resides within the WebServerVM into separate Lambda functions.

**API Gateway Implementation**: Implementation of API Gateway thus allowing the entry of incoming HTTP requests by directing them to the Lambda functions for a particular purpose. Define stages (dev, prod etc) for the purpose of wind up testing and introducing to the audience.

**Step 2: Migrate Database to Amazon RDS with Multi-AZ**

**Database Migration Planning**: Even though the database stored in a virtual machine may be challenging, AWS DMS will assist to overcome this limitation by migrating the active MySQL instance to Amazon RDS with minimal downtime.

**Create RDS Instance**: Launch an Amazon RDS instances for a Multi-AZ MySQL Database. Set up automated logs backups and monitoring for the instance.

**Database Cutover**: In the cutover period, switch over API application from the existing RDS instance to the newly created RDS instance. Test whether the Multi-AZ replication is working as expected.

**Step 3: Incorporate Redundancy and Failover**

**Configuration of Route 53**: Leverage the features of Route 53 to perform health checking and failover policies so that traffic is always routed to healthy systems and resources.

**Test Failover Mechanisms**: Degrade all instances to one AZ and verify there is sufficient seamless failover of the RDS instance on standby to primary without any loss of data.

**Step 4: Monitoring and Optimization**

**Establish Monitoring**: Apply CloudWatch to track metrics of Lambda functions (such as usage duration or number of failures) as well as RDS metrics. Activate alarms to provide for scaling and alerting.

**Turn on Debugging Features using AWS X-Ray**: Enable X-ray in order to trace the route of a user request through various Lambda functions for easier performance optimisation.

# Conclusion

This approach makes it possible to move the application to an elastic, highly-available, cost-effective, serverless architecture with AWS Lambda and Multi-AZ RDS. Timely management and refactoring will allow efficient business operations for the management team as the system is designed to withstand an average of six hours of down time.
