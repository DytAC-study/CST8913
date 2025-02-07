### **Multi-Region Deployment with Load Balancing - High-Level Design (HLD)**

#### **1. Solution Diagram**

![image-20250206204535596](C:\Users\yunti\AppData\Roaming\Typora\typora-user-images\image-20250206204535596.png)

------

#### **2. Target Architecture**

As we need to ensure high availability and meet the 6-hour downtime requirement, the application is deployed in 2 regions with the following components:

- **WebServerVM Redundancy (At least three per Region)**:
  - At least three(depends on assessment process)  VMs for webserver is created inside each region.
  - Using Azure Virtual Machine Scale Sets to automatically scale and manage instances based on demand.
- **SQLVM Redundancy (At least three per Region)**:
  - At least three(depends on assessment process)  VMs for SQL server is created inside each region.
  - Using Azure SQL Always On Availability Groups for high availability and automatic failover.
  - Geo-Replication is configured for disaster recovery, ensuring data consistency across regions.
- **Load Balancing**:
  - Azure Traffic Manager: Routes traffic to WebServerVM instances across regions.
  - Azure Application Gateway: Provides secure traffic distribution within each region and enhances web application security.
  - Azure Load Balancer: distribute traffic between WebServerVM and SQLVM instances.
- **Geo-Redundant Storage (GRS)**:
  - Ensures data is stored across multiple regions for disaster recovery.
  - Provides automatic failover and data consistency in case of primary region failure.
- **Failover Mechanism**:
  - If one region experiences failure, Azure Traffic Manager will route web traffic to the alternative region.
  - The SQL database in SQLVMs uses Azure SQL Always On Availability Groups with automatic failover to prevent data loss.

## 3. Migration Steps

### Step 1: Assessment

1. Investigate the assets using for this web service.
2. Using Azure readiness tool to assess if on-premises machines are ready for migration
3. Using Azure sizing to estimate the size and numbers of VMs needed.

### Step 2: Deploy VM on Azure cloud

1. Deploy WebServerVM and SQLVM in the primary region using Azure VMSS and Azure SQL Always On Availability Groups.
2. Deploy everything to the secondary region using Azure Site Recovery.
3. Use Azure Database Migration Service (DMS) to perform an online or offline migration of the on-premises SQL database to Azure SQLVM.
4. Configure SQL Server transactional replication to continuously synchronize data from the on-premises SQL Server to Azure SQLVM until full migration is completed.

### Step 3: Configuration of Load Balancers

1. Deploy Azure Traffic Manager to distribute traffic between WebServerVM instances in different regions.
2. Set up Azure Application Gateway in each region to manage intra-region traffic and enhance security.
3. Deploy Azure Load Balancer within each region to distribute traffic among WebServerVM and SQLVM instances.
4. Enable monitor tools and policies to help monitor each services' status and keep them in control.
5. Test failover by simulating a regional failure.

### Step 4: Implementation of Database Replication and Failover

1. Configure SQLVM for Azure SQL Always On Availability Groups to ensure high availability within the region.
2. Implement automatic failover using Geo-Replication between regions.
3. Test database failover by shutting down the primary SQLVM and verifying seamless transition to the secondary SQLVM.
4. Switchover to the SQLVM and disconnect the on-premises SQL Server.

## Conclusion

We're using two regions to prevent disaster or other failure on one region, and using Azure Traffic Manager, Azure Application Gateway and Azure Load Balancer to distribute traffic in different levels to make sure the network stability. When VM is facing downtime, load balancer will direct traffic to those healthy VMs and monitor will inform us about this issue. To ensure data security, we're implementing Geo-Redundant Storage and Azure SQL Always On Availability Groups to provide high availability and auto failover.