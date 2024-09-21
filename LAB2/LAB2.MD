                                                  LAB 2

+-------------+         +-------------+         +-------------+
|             |         |             |         |             |
|   React     |  <--->  |   Flask     |  <--->  | PostgreSQL  |
| Frontend UI |         | Web Server  |         |  Database   |
|             |         |             |         |             |
+-------------+         +-------------+         +-------------+

Frontend-   It will provide the user-interface and will handle client side interactions.
            It will communicate with the backend Flask server via HTTP requests.

Web Server- It will interact with PostgreSQL database to store and retrieve data.
            It will implement application's business logic.

PostgreSQL- It will store the persistent data for the application.
            It will provide scalable database solution for storing structured data.


                                             IAAS DEPLOYMENT

     +----------------------------+ 
     | [User] --> [Load Balancer] |
     |           |                |
     |    [Web Server VM]         |
     |        (React)             |
     |           |                |
     |   [Application Server VM]  |
     |         (Flask)            |
     |           |                |
     |     [Database VM]          | 
     |      (PostgreSQL)          |
     +----------------------------+  
Load Balancer- It will distribute incoming traffic across web servers for better availability and scalability.
Virtual Network- It will connect all components securely within a private network.
Security Groups- It will Control Inbound and Outbound traffic to VMs.
Monitoring- Cloud-native tools can be used for performance monitoring and log aggregation.
IaaS platforms often provide automated security configuration assessments, patching, and database functions, reducing the risk of human error


                                             PAAS DEPLOYMENT
+-----------------------------------------+ 
|    [User] --> [Cloud Load Balancer]     |
|                  |                      |
|    [Managed Web Hosting Service]        |
|          (React Static Files)           |
|                  |                      |
|    [App Container Service / Serverless] |
|               (Flask)                   |
|                  |                      |
|    [Managed Database Service]           |
|            (PostgreSQL)                 |
|                                         |
+-----------------------------------------+

The PaaS approach significantly reduces operational overhead by using cloud-native services, allowing developers to focus more on application logic rather than infrastructure management. It also provides built-in scalability and high availability features, though at the cost of some flexibility compared to the IaaS approach. PaaS providers typically manage system updates and security patches, which ensures systems remain current and protected.

