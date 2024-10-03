
# Target Architecture Overview

This shift-and-move strategy will ensure high availability, redundancy, and fault tolerance by configuring the WebServerVM and SQLVM within a multi-region cloud environment. Key components of this architecture include replicated virtual machines (VMs) for both web and database tiers, load balancers to distribute traffic, and automatic failover mechanisms.

## Target Architecture Diagram

```
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
```

## Key Components

- **Multi-Region VM Replication**: The two VMs, WebServerVM (frontend) and SQLVM (backend), will be replicated in two or more cloud regional zones.

- **Load Balancers**: Global load balancers will direct traffic to the nearest healthy region, ensuring balanced traffic distribution across regions.

- **Automatic Failover**: In case of regional failure, WebServerVMs and SQLVM will fail over to the other active region. SQLVM in the secondary region will assume the role of the backend if the primary region fails.

- **Database Replication**: SQLVM will use asynchronous replication between the primary and secondary regions. In case of failover, minimal data loss will occur due to the asynchronous replication.

## Architecture Assumptions

- **Global Load Balancer**: This device is positioned above both regions and connects multiple client edges. It routes traffic based on geographical distance and region health. If Region A fails, Region B will take over the traffic, minimizing downtime.

- **WebServerVM Replication**: The static content of the application will be mounted on WebServerVM instances in both Region A and Region B. Load balancers will distribute traffic to healthy instances, and static content will be synchronized between the regions.

- **SQLVM Replication and Failover**: The SQLVM A (primary) database will be asynchronously replicated with SQLVM B (secondary). If Region A fails, SQLVM B will be promoted to primary, ensuring access to the latest data with minimal loss.

- **Failover Mechanisms**: Both WebServerVM and SQLVM support automatic failover. The global load balancer will detect if any VMs become unavailable and re-route traffic accordingly. If SQLVM A fails, SQLVM B will automatically be promoted to primary, and a new replica will be created in the failed region upon recovery.

## Migration Steps

1. **Replication of Virtual Machines Across Regions**:
   - Deploy duplicate WebServerVM and SQLVM instances in Region B.
   - Use the cloud provider’s native tools (e.g., AWS EC2 AMI, Azure VM Snapshot) to replicate the VMs' configurations across regions.
   - Set up automatic replication of static content for WebServerVM across multiple regions.

2. **Configuration of Load Balancers**:
   - Implement regional load balancers in both Region A and Region B.
   - Configure global traffic distribution using a global load balancer with a "least load" primary selection feature.
   - Implement health checks for VMs in both regions to enable automatic failover in case of failure.

3. **Database Replication and Failover Provisions**:
   - Set up asynchronous replication between SQLVM A (primary) and SQLVM B (secondary).
   - Ensure failover provisions where, in case of Region A downtime, SQLVM B assumes the primary role.
   - Establish monitoring and alert mechanisms for database replication and performance issues.

## Conclusion

This design facilitates an effective lift-and-shift migration to the cloud while ensuring high availability, automatic failover, and reduced mean downtime across regions. By replicating the front-end and back-end systems and using global load balancers, the application can continue operating even during prolonged regional outages, staying within the 6-hour downtime limit.
```
