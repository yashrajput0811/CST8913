
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

- **SQLVM Replication and Failover**: SQLVM A will asynchronously replicate with SQLVM B. It is cautioned that if Region A were to fail, that SQLVM B would then be promoted to primary ensuring that there will be no or very little data loss of the best available data.

- **Failover Mechanisms**: Both WebServerVM and SQLVM have continuity of operations through automatic failover features. It helps that the global load Masquée gets to know when there are VM’s which are not available and ensures that the traffic is redirected in the right way.

## Migration Steps

1. **Replication of Virtual Machines Across Regions**:
   - Deploy the duplicate WebServerVM and SQLVM instances in Region B.
   - Use the cloud provider’s native tools to replicate the VMs' configurations across regions.
   - Set up automatic replication of static content for WebServerVM across multiple regions.

2. **Configuration of Load Balancers**:
   - Configuration of Load Balancers: Deploy regional load balancers in each Region A and Region B.
   - Distribution of global traffic for the same services offered in two regional sites with a global load balancer functioning in primary load.
   - Exchange VMs in both regions with working health checks to facilitate automatic recovery procedures when one region fails.

4. **Database Replication and Failover Provisions**:
   - Connect SQLVM A (primary) to SQLVM B (secondary) through asynchronous replication.
   -  Provide for provisions for failover where region A is down and SQLVM B takes up the primary position.
   -  Provisions for monitoring as well as alerts for database replication and database performance issues.
## Conclusion

This design enables efficient lift-and-shift cloud migration with minimal downtime, operational failover capability or regional unavailability and minimal mean down time. By employing Global Load Balancers and replicating both the front-end and back-end systems, the application can remain functional in the 6 hour downtime region even in case of lengthy regional outages.
```
