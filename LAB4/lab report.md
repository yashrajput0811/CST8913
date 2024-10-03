
# Target Architecture Overview

This approach will guarantee maximum uptime, redundancy and fault-tolerance by deploying the WebServerVM and SQLVM in a multi-regional cloud architecture. Essential elements of such an architecture include replicated web and database virtual machines, load balancers and automatic failure epicycle recovery methods.

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

- **Multi-Region VM Replication**: The two VMs, WebServerVM (frontend) and SQLVM (backend), shall be replicated in two or more cloud regional zones encompassing the cloud VMs.

- **Load Balancers**: Global load balancers will point the traffic towards the healthy nearest region and therefore traffic will be evenly spread across various regions.

- **Automatic Failover**: In case of regional failure, WebServerVMs and SQLVM will be failed over to the other active region. In case the primary region fails, it will be assumed by the SQLVM of region 2 that there is only SQL backend.

- **Database Replication**: SQLVM shall perform asynchronous replication between primary and secondary regions. However, as elegant as it sounds, in case any fail over, there will be loss of data due to asynchronous replication.

## Architecture Assumptions

- **Global Load Balancer**: This device is placed above both regions and connects a number of client edges. This device allows for cutting traffic by region with respect to geographical distance and region health. In case Region A stops functioning, Region B will take over the traffic and therefore avoid any service downtime.

- **WebServerVM Replication**: Application static content will be mounted onto WebServerVM instances located in Region A and in Region B as well.

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
