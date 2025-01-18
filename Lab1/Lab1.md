# Lab1

Name: Yuntian Du 

Student ID: 041158542

**Story： Bosch delivers supply chain efficiencies using Java on Azure**



## Question 1：What are the company's motivators of migration to the cloud?

In this case, Bosch developed a service named "Track and Trace", this service uses a lot of IoT devices tracking goods or containers all over the world. The majority team members are java (Spring Boot) developers, they tend to free their hands form dealing problem related to infrastructures, Kubernetes or system administration. They just don't want to waste time on things other than Java development.

Also, it is their desire to expand or update their service in a rapid speed with the least change in their architecture, so they choose to migrate to the cloud using microservice architecture. Finally they chose Azure Spring Apps for their whole program.

All in all, I believe this company's motivators are: Increase in business agility, Scaling to meet
geographic and market demands, Datacenter exit and Reduction of disruptions and improvement of IT stability.



## Question 2: What questions will you ask to understand more about the company infrastructure?

As told in the story, Track and Trace includes 15 microservices developed using different framework, so they don't need to care about the server type or switches and routers. But if we would like to know more about their infrastructure they were using before migrating to Azure cloud, I would like to ask them about:

1. Their network structure and network service provider
2. The IoT devices they're using
3. Their server's configuration (model and number of CPU, memory and hard disk)
4. Are they using their own private network disk?
5. The database they're using
6. The virtualization software they're using
7. The operating system of the container their apps are running on
8. Is there any special hardware on their server



## Question 3: Produce a RACI matrix for the migration stakeholders

| **Task**                               | **Project Manager** | **Development Team** | **Cloud Architect** | **Azure Administrator** | **QA Team** | **Stakeholders** |
| -------------------------------------- | ------------------- | -------------------- | ------------------- | ----------------------- | ----------- | ---------------- |
| Monitor and optimize post-migration    | A                   | C                    | R                   | R                       | C           | I                |
| Assess current application state       | A                   | R                    | C                   | I                       | I           | I                |
| Design Azure Spring architecture       | A                   | C                    | R                   | C                       | I           | I                |
| Set up Azure Spring environment        | A                   | C                    | R                   | R                       | I           | I                |
| Refactor applications for Azure Spring | I                   | R                    | C                   | I                       | C           | I                |
| Test applications in Azure Spring      | A                   | C                    | C                   | C                       | R           | I                |



## Question 4: Describe the most likely migration approach that will suit the chosen company.

1. First, we need to discover the whole architecture and plan everything. We need to figure out the dependencies for each part and also find out which part of the app needs adjustments and which could be directly delivered to the cloud. Since they only have Spring Cloud Apps, there will not be many changes.
2. Then we need to use tools from Azure to migrate a minimize-sized service of our App for further tests, if there's any issue, Azure administrators or engineers will need to join and fix it.
3. After migration, there'll be a test period to see if every part of the app works fine. If problems pop out, we need to adjust it. If everything works fine, then we're ready to scale up.
4. We expand our deployment to the scale that can hold our business, and run a test to see if it is stable.
5. Finally we turn the traffic of our App to the cloud environment.



## Question 5: Produce a high level schedule for the migration process.

They had a 2-month's timetable for this migration.

Week 1: discover the whole architecture， dependencies and plan for migration.

Week 2-3: migrate the minimal service to the cloud (work with Azure team)

Week 4-6: Test the performance and reliability

Week 7-8: Expand and cut the traffic to cloud.



## Question 6: What are the main decision criteria for the chosen company?

Mainly they want to no-longer care about infrastructures, so they can focus on their app rather than doing maintenance work, and also they're interested that their apps could scaling to meet geographic and market demands.