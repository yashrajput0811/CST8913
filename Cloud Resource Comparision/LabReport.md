# Cloud Resource Comparison Table

| #  | Description                                                                                                    | AWS (Service Name)             | Azure (Service Name)            | Google Cloud (Service Name)           |
|----|----------------------------------------------------------------------------------------------------------------|--------------------------------|---------------------------------|---------------------------------------|
| 1  | A compute service that provides scalable virtual machines for running applications.                            | EC2                            | Virtual Machines                | Compute Engine                        |
| 2  | An object storage service used to store and retrieve data, commonly used for backups and static website content| S3                             | Blob Storage                    | Cloud Storage                         |
| 3  | A managed relational database service that supports multiple database engines like MySQL, PostgreSQL, and SQL. | RDS                            | Azure SQL Database              | Cloud SQL                             |
| 4  | A serverless compute service to run code in response to events without managing servers.                       | Lambda                         | Azure Functions                 | Cloud Functions                       |
| 5  | A virtual private network service that allows you to create isolated networks within the cloud provider.       | VPC                            | Virtual Network (VNet)          | Virtual Private Cloud (VPC)           |
| 6  | A content delivery network (CDN) service to deliver data, videos, applications with low latency.               | CloudFront                     | Azure CDN                       | Cloud CDN                             |
| 7  | A managed NoSQL database service designed for low-latency, high-scale applications.                            | DynamoDB                       | Cosmos DB                       | Firestore/Datastore                   |
| 8  | A block storage service for use with virtual machines, offering persistent storage for data.                   | EBS                            | Managed Disks                   | Persistent Disks                      |
| 9  | A managed container orchestration service based on Kubernetes.                                                 | EKS                            | AKS (Azure Kubernetes Service)  | GKE (Google Kubernetes Engine)        |
| 10 | A service for managing user access and encryption keys to secure cloud resources.                              | IAM & KMS                      | Azure Active Directory & Key    | IAM & Cloud KMS                       |
| 11 | A platform that automates application deployment and scaling without needing to manage infrastructure.         | Elastic Beanstalk              | App Service                     | App Engine                            |
| 12 | A service that provides monitoring and logging of applications and infrastructure.                             | CloudWatch                     | Azure Monitor                   | Operations Suite                      |
| 13 | A domain name system (DNS) service that routes traffic globally and translates domain names to IP addresses.   | Route 53                       | Azure DNS                       | Cloud DNS                             |
| 14 | A load balancing service that distributes incoming traffic across multiple targets.                            | Elastic Load Balancing (ELB)   | Azure Load Balancer             | Cloud Load Balancing                  |
| 15 | A service that automatically scales your cloud infrastructure based on demand.                                 | Auto Scaling                   | Azure Autoscale                 | Autoscaler                            |
| 16 | A message queuing service to send and receive messages between different components.                           | SQS                            | Azure Queue Storage             | Pub/Sub                               |
| 17 | A managed real-time data streaming service that collects and processes large amounts of data.                  | Kinesis                        | Azure Event Hubs                | Dataflow                              |
| 18 | A fully managed, highly scalable data warehouse service for large-scale queries.                               | Redshift                       | Azure Synapse Analytics         | BigQuery                              |
| 19 | A service that automates the execution of workflows and integrates cloud services in steps.                    | Step Functions                 | Azure Logic Apps                | Cloud Composer                        |
| 20 | A service that integrates multiple data sources, enabling data migration, transformation, and movement.        | AWS Glue                       | Azure Data Factory              | Dataflow                              |
| 21 | A data catalog and governance service that helps manage metadata, supporting compliance and security.          | AWS Glue Data Catalog          | Azure Purview                   | Data Catalog                          |
| 22 | A set of machine learning and AI services providing pre-built models and APIs.                                 | SageMaker                      | Azure Machine Learning          | AI Platform                           |
| 23 | A service to define and deploy infrastructure using code.                                                      | CloudFormation                 | Azure Resource Manager          | Deployment Manager                    |
| 24 | A fully managed CI/CD service that automates building, testing, and deployment of applications.                | CodePipeline                   | Azure DevOps                    | Cloud Build                           |
| 25 | A desktop as a service (DaaS) offering for deploying virtual desktops and accessing them remotely.             | WorkSpaces                     | Azure Virtual Desktop (AVD)     | Cloud Virtual Desktop                 |
| 26 | A backup and disaster recovery service that protects data by creating backups.                                 | AWS Backup                     | Azure Backup                    | Backup & DR Service                   |
| 27 | A service for big data analytics, allowing organizations to store, process, and analyze large datasets.        | EMR                            | HDInsight                       | Dataproc                              |
| 28 | A file storage service for storing and sharing files with users.                                               | EFS                            | Azure Files                     | Filestore                             |
| 29 | A service for transcoding, processing, and streaming media content such as video and audio.                    | Elastic Transcoder             | Azure Media Services            | Transcoder API                        |
| 30 | A real-time communication service for sending notifications, emails, and text messages to users.               | SNS                            | Azure Notification Hubs         | Firebase Cloud Messaging              |

---

# Summary Report

## Similarities Across AWS, Azure, and Google Cloud Services
Cloud services across AWS, Microsoft Azure, and Google Cloud often share significant similarities.
This is because of a similar denominator infrastructure nature of these three platforms which is
targeting specifically large enterprises and developers. Many services such as computing storage
or networking resources have identical analogs across these platforms.
In particular, AWS EC2, Microsoft Azure Virtual Machines and Google Compute Engine all allow
launching and operating flexible virtual computers.
The same way S3, Blob Storage and Cloud Storage provide the objects storage services
regardless of the content purpose.

Managed database services also follow this pattern, with AWS RDS, Azure SQL Database,
and Google Cloud SQL providingcomparable relational database management with support
for popular engines like MySQL and PostgreSQL.

## Differences and Unique Features
Although the main purpose of these services is usually the same, the platforms could contain
differences in feature sets and implementation details. For example, two BigQuery and AI
Platform from Google Cloud are oriented to the data science and real
time analytics, thus contributing to the specialization of the firm in artificial
intelligence and big data analysis using itscloud infrastructure that has the tools for these
services. At the same time, Azure is very much devoted to the hybrid cloud
strategies and this is very much so reflected in Azure Arc, which allows for the management
of Azure services on local machines. AWS is known for its breadth of services and deep
customization capabilities. For example, AWS Lambda provides advanced 
serverless compute options, often regarded as more flexible than its 
Azure and Google Cloud counterparts.

## Naming Conventions
The title given to these providers usually show what they are all about and are professionally appealing.
For instance, descriptive titles are used by AWS: elastic is used to describe scaling 
(In this case, Elastic Load Balancing), while more or less plain,
thus technology-oriented names are used by Azure (for example, Virtual Machines, or Load Balancer).
In Google Cloud, naming is also often relatively simple and neat.
Where, however, the naming convention reflects the purpose of the service (for example, Compute Engine, or Cloud Storage).
These patterns of naming may make the services offered by AWS appear complicated but quite flexible, 
while Azure and Google Cloud and more or less simple and easy to remember.

## Conclusion
Despite having similar services in virtually all areas such as AWS, Azure and Google Cloud, what sets them apart are the
specific ways of implementing features.
And the unique advantages of each such as the sheer volume of services offered by AWS, the hybrid cloud strengths of Azure,
or the data and AI focus of Google Cloud.
These similarities and differences are essential of understanding in making choices on cloud solutions to fit particular 
organizational or application needs.
