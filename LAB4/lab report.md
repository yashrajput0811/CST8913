                                                          High-Level Design (HLD) for Lift-and-Shift Migration


Target Architecture Overview:

This shift-and-move strategy will make certain that high availability, redundancy and fault tolerance principles will be achieved by configuring the WebServerVM and SQLVM within a multi-region cloud environment. Key components of this architecture include replicated virtual machines (VMs) for both web and database tiers, load balancers to distribute traffic, and automatic failover mechanisms.

Target Architecture Diagram:
                                  ┌────────────────────────┐
                                  │   Global Load Balancer │
                                  └────────────────────────┘
                                              │
             ┌────────────────────────────────┴────────────────────────────────┐
             │                                                                 │
   ┌─────────────────────────────┐                                ┌──────────────────────────────┐
   │ Region A (Primary)          │                                │ Region B (Secondary)         │
   └─────────────────────────────┘                                └──────────────────────────────┘
             │                                                                 │
   ┌─────────────────────────────┐                                ┌──────────────────────────────┐
   │  Load Balancer A            │                                │  Load Balancer B             │
   └─────────────────────────────┘                                └──────────────────────────────┘
             │                                                                 │
   ┌─────────────────────────────┐                                ┌──────────────────────────────┐
   │ WebServerVM A               │                                │ WebServerVM B                │
   │ (Frontend - Static Content) │                                │ (Frontend - Static Content)  │
   └─────────────────────────────┘                                └──────────────────────────────┘
             │                                                                 │
   ┌─────────────────────────────┐                                ┌──────────────────────────────┐
   │ SQLVM A                     │                                │ SQLVM B                      │
   │ (Primary DB)                │                                │ (Secondary DB - Replication) │
   └─────────────────────────────┘                                └──────────────────────────────┘
             │                                                                 │
   ┌─────────────────────────────┐                              ┌──────────────────────────────┐
   │ Asynchronous DB Replication │──────────────────────────────│ Asynchronous DB Replication  │
   └─────────────────────────────┘                              └──────────────────────────────┘


Key Components:

Multi-Region VM Replication: The two VMs: the WebServerVM (frontend) and SQLVM (backend), will be replicated in two or more cloud regional zones.

Load Balancers: Global load balancers will be employed to expand the regions and direct traffic requesting regions to the closest possible healthy region.

Automatic Failover: When regional failure occurs and in the case of the primary region, WebServer VM regions will fail over to the other active region and
terms in the case of the SQLVM region for use as a backend.

Data Base Replication: SQLVM will ensure that upon failover, the data lost will be negligible as it will use asynchronous replication only between the primary
and secondary regions.

Architecture Assumptions:

Global Load Balancer: This device is situated above the entire region with two regions located and connects multiple edges or views of clients normal way back
at times it connects to a traffic received to that access global load balancer which checks up traffic on geographical distance or health of each region.
If region A has failed, then region B will take all the traffic and in that way use pauses are reduced.

WebServerVM Replication: The application static content is mounted on the WebServerVM instances in Region A and in Region B such that both of them are WebServerVM. 
The load balancers will spread the traffic on healthy instances across the regions and static objects will be synchronized between the regions.

SQLVM Replication and Failover: The database in SQLVM A primary, will at some point in time be asynchronously replicated with SQLVM B secondary. 
In case where failure happens in Region A, SQLVM B will be promoted to primary in order to avoid loss of data as a result of asynchronous replication. 
By replicating the most recent SQL VM database information streams across regions, more current data access is achieved.

Failover Mechanisms: Within the virtual machine of the webserver, as well as sql virtual machine, automatic failover feature is supported. The global load balancer can find out if any of the VMs in the region becomes unavailable and can re-route the traffic to other regions in such cases. As for the failure in the primary instance, if such occurs, SQLVM B will automatically get promoted to the primary node, while a new replica will be created in the failed region upon restoration of power.

Migration Steps:

Replication of Virtual Machines across Regions:
Deploy duplicate WebServerVM and SQLVM instances in secondary region B because it is referred to as Region B.
Utilize the cloud provider’s inbuilt native tools for performing the replication of the virtual machine images,
i.e. AWS EC2 AMI and Azure VM Snapshot to prevent deviations in the regions configuration.
To WebServerVM, set up automatic replication of static content that spans more than one region.

Changing the configuration of Load balancers:
Implement regional load balancers in Region A and Region B.
Configure global traffic distribution across regions using a Global Load balancer with Least Load Primary Selection Feature.
Implement health checks for the VM's in both regions so that automatic failover can be carried out in case of failure.

Database replication and fail over provisions:
Conduct asynchronous replication of the database in SQLVM A (active) and SQLVM B (standby).
Ensure that there are fail-over provisions where in case there is a down time for Region A SQLVM B assumes the primary position.
Establish monitoring and alerts for issues related to database replication and issues in performance of the application.

Conclusion:

This level of design enables an effective lift-and-shift application migration to the cloud retaining high availability, automatic switchover,
and reduced mean downtime across regions. Since the systems of the front and back ends and global load balancer are duplicated, 
the application can be operated despite well over 6- hours of nonstop downtime.
