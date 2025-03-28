# Migration Plan for Tailwind OpenCare Patient Self-Booking System to Azure

## 1. Discovery and Tooling Setup

### **Tools for Discovery** 

Azure Migrate: Provides an appliance (lightweight VM) that can perform agentless discovery of VMware or Hyper-V VMs, or agent-based discovery for physical/Linux servers. It will gather an inventory of the existing environment, including server configurations and performance data. This tool is preferred for comprehensive discovery because it automatically collects details needed for assessment (OS, software, dependencies, performance) in one solution, and it’s agentless in many scenarios (meaning minimal installation on existing servers).



### **Data to Collect**

- ***System Info**:* Operating system version/patch level and hardware configuration of each server.
- ***Software Inventory**:* Installed applications and services (Node.js runtime version, NGINX version, PostgreSQL version, Redis version, any libraries or third-party components).
- ***Application Topology**:* Document which servers talk to each other and on what ports (e.g., NGINX → Node.js on port 3000, Node.js → PostgreSQL on 5432).
- ***Performance Metrics**:* Collect CPU utilization, memory usage, disk I/O, and network throughput for each server (Azure Migrate captures this continuously during discovery). This data over time will be used to right-size Azure resources. For example, note if the Node.js API servers are typically at 50% CPU on 2 cores, or if the PostgreSQL server handles X transactions/minute and requires Y IOPS.
- ***Current Redundancy/Load:*** Note how the two NGINX and two Node.js servers are load-balanced and any replication for PostgreSQL or Redis.



### **Credential and Permission Handling**

- Use Azure Key Vault to store and manage sensitive information, use Access Policies to make sure only authorized services and users can retrieve specific secrets.
- Follow the Least Privilege Principle to make sure only the necessary permission is assigned for certain tasks.

## 2. Assessment Planning

### **Azure Migrate Assessment Configuration**

- **Performance History**: Use Azure Migrate to collect performance data for one month with a peak utilization percentile of 80% to recommend the size of Azure resources. It will offer recommended VM sizes and managed disks based on actual server requirements. 
- **Performance-Based Sizing** **strategy**:  Utilizes actual utilization data—such as CPU, memory, and disk IOPS—from the on-premises environment to determine appropriate Azure resource allocations. By analyzing historical performance metrics, this method ensures that Azure resources are right-sized, balancing performance requirements with cost efficiency.

### **Target Azure Configuration**

- **Region**: If most users and the on-prem datacenter are in a specific country/region, select the **closest Azure region** to minimize latency. If Tailwind OpenCare is based in Ontario, **Canada Central** (Toronto) or **Canada East** would be logical region choices. Ensure the region supports all needed PaaS services: Azure App Service, Azure Database for PostgreSQL, Azure Cache for Redis.

- **Compute Type:** Utilize Azure Virtual Machines for the application servers, choosing VM sizes based on assessment findings.

  **Storage Type:** Premium SSDs for the database server to ensure high I/O performance and Standard SSDs for application servers to balance performance and cost.

### **Validation with Stakeholders**

I will conduct meetings with technical and business stakeholders to show them the assessment results to make sure the target Azure Configuration match their performance expectations and budget constraints. Then I will share detailed assessment reports and obtain formal approval before proceeding to the migration phase.

## 3. Dependency Analysis and Optimization

### **Agentless Dependency Mapping**

Using Azure Migrate’s dependency analysis, we will map out communication between servers. We enable this via the Azure Migrate appliance and by deploying the Dependency agent on each server for more exact information. Azure Migrate will visualize connections between the on-prem servers over a specified period:

- Expect to see connections such as:
  - User → Load Balancer on port 443
  - Load Balancer → NGINX on HTTP/HTTPSNGINX → Node.js on port 3000
  - Node.js → PostgreSQL on port 5432 
  - ...

### Data Cleaning

Not all detected “dependencies” need to move to Azure or even be accounted for in the new design. We need to filter out environment-specific or irrelevant connections:

- In the dependency map, filter out infrastructure dependencies, like Node.js server communicating with on-premises logging or monitoring system, which will be replaced by Azure Monitor.
- We need to determine if any component can be decommissioned instead of migrated. Some servers may no longer receive traffic or inactive, we need to get rid of them. Some of the services will be replaced by PaaS, which also needs to be removed.

### Key Metrics Identification

**Criticality Assessment:**

Evaluating the importance of each server to business operations

- **Infrastructure Mapping:** Create a comprehensive diagram of the current infrastructure, detailing all servers and their interconnections. This helps us understand the system's architecture and identify potential points of failure. 
- **Business Process Analysis:** List all business processes supported by the Patient Self-Booking System and determine which servers are integral to these processes.
- **Impact Evaluation:** Assess the potential consequences of each server's failure, considering factors like financial loss, reputational damage, regulatory compliance issues, and customer trust. Servers whose failure would significantly disrupt operations or violate compliance standards should be classified as high criticality. [Redjack](https://redjack.com/resources/the-key-to-cyber-resilience-identifying-critical-business-functions-for-success?utm_source=chatgpt.com)

**User Base Identification:**

Understanding the number of users affected by each component involves:

- **Usage Metrics Collection:** Gather data on user interactions with each server, including metrics like daily active users, transaction volumes, and peak usage times.
- **Stakeholder Consultation:** Collaborate with departments such as customer service and IT support to gain insights into user engagement and identify components that are heavily relied upon.

**Service Level Agreements (SLAs):**

Documenting existing SLAs ensures that post-migration performance meets organizational commitments.  Examine current agreements related to uptime guarantees, response times, and data recovery objectives. Ensure that the migration plan adheres to these SLAs.

**Backup Requirements:**

Establishing appropriate backup frequency and retention policies for each component:

- **Data Classification:** Categorize data based on sensitivity and importance, distinguishing between critical patient information and less sensitive data.
- **Retention Policy Development:** Define how long different types of data should be retained, considering legal requirements, compliance standards, and business needs.
- **Backup Strategy Implementation:** Adopt a 3-2-1 backup strategy, maintaining three copies of data on two different media types, with one copy stored off-site.

## 4. Server Grouping and Migration Waves

Tailwind OpenCare is finalizing the prioritization of migration waves for their Patient Self-Booking System, considering factors such as workload criticality, dependencies, and business requirements. Based on interviews and dependency mapping, the server groups have been refined to align with the application's multitier architecture and interdependencies. The updated server groupings are:

- **Server Group 1:** PostgreSQL database server and Redis caching server.
- **Server Group 2:** Node.js API servers (Backend).
- **Server Group 3:** NGINX web servers (Frontend).

**Prioritization and Wave Planning:**

1. **Wave 1:** Migrate Server Group 1, which includes the PostgreSQL database to Azure Database for PostgreSQL and the Redis instance to Azure Cache for Redis, ensuring that backend services have access to the necessary data stores in the Azure environment.
2. **Wave 2:** Migrate Server Group 2, consisting of the Node.js API servers. The migration of backend services ensures that the application logic can interact seamlessly with the migrated databases and caching services.
3. **Wave 3:** Migrate Server Group 3, which includes the NGINX web servers. Moving the frontend components last ensures that the user interface is updated to connect with the newly migrated backend services, minimizing potential disruptions to the user experience.

**Precautions:**

- **Firewall Rules:** Copy on-premises firewall configurations within Azure Network Security Groups to maintain existing security postures. 
- **Load Balancer Configurations:** Using Azure Load Balancer or Azure Application Gateway to distribute incoming traffic across the migrated servers, ensuring load distribution and high availability.
- **IP Address Changes:** Update DNS records to reflect the new IP addresses assigned to Azure resources.

## 5. Migration Plan Documentation (Step-by-Step Execution)

**Step-by-Step Migration Using Azure Services:**

1. **Database Migration:**
   - Set up a managed PostgreSQL instance in Azure.
   - Use Azure Database Migration Service to transfer data from the on-premises PostgreSQL server to Azure with minimal downtime.
2. **Redis Migration:**
   - Create a Redis cache instance in Azure.
   - Export data from the on-premises Redis instance and import it into Azure Cache for Redis.
3. **Backend Migration:**
   - Deploy Azure VMs to host the Node.js API servers.
   - Transfer application code and configurations to the Azure VMs.
4. **Frontend Migration:**
   - Deploy Azure VMs to host the NGINX web servers.
   - Transfer web content and configurations to the Azure VMs.
5. **Load Balancing:**
   - Set up load balancing to distribute incoming traffic across the NGINX web servers.
6. **DNS Updates:**
   - Modify DNS entries to point to the new Azure-hosted web servers, ensuring users are directed to the correct IP addresses.
7. **Backup Validation:**
   - Configure Azure Backup to perform daily backups of critical data, ensuring compliance with data protection policies.
   - Regularly test backup restorations to verify data integrity and availability.

**Roles of Application Owners:**

- **Pre-Migration Testing:** Conduct thorough testing in a staging environment to validate application functionality and performance before the production migration.
- **Post-Migration Validation:** After migration, perform comprehensive testing to ensure all components function as expected in the Azure environment.