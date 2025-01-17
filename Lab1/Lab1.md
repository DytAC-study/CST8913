# Lab1

Story： Bosch delivers supply chain efficiencies using Java on Azure



## Question1：What are the company's motivators of migration to the cloud?

In this case, Bosch developed a service named "Track and Trace", this service uses a lot of IoT devices tracking goods or containers all over the world. The majority team members are java (Spring Boot) developers, they tend to free their hands form dealing problem related to infrastructures, Kubernetes or system administration. They just don't want to waste time on things other than Java development.

Also, it is their desire to expand or update their service in a rapid speed with the least change in their architecture, so they choose to migrate to the cloud using microservice architecture. Finally they chose Azure Spring Apps for their whole program.

All in all, I believe this company's motivators are: Increase in business agility, Scaling to meet
geographic and market demands, Datacenter exit and Reduction of disruptions and improvement of IT stability



## Question2: What questions will you ask to understand more about the company infrastructure?

As told in the story, Track and Trace includes 15 microservices developed using different framework, so they don't need to care about the server type or switches and routers. But if we would like to know more about their infrastructure they were using before migrating to Azure cloud, I would like to ask them about:

1. Their network structure and network service provider
2. The IoT devices they're using
3. Their server's configuration (model and number of CPU, memory and hard disk)
4. Are they using their own private network disk?
5. The database they're using
6. The virtualization software they're using
7. The operating system their apps are running on
8. Is there any special hardware on their server



## Question3: Produce a RACI matrix for the migration stakeholders

T

| **Task**                               | **Project Manager** | **Development Team** | **Cloud Architect** | **Azure Administrator** | **QA Team** | **Stakeholders** |
| -------------------------------------- | ------------------- | -------------------- | ------------------- | ----------------------- | ----------- | ---------------- |
| Monitor and optimize post-migration    | A                   | C                    | R                   | R                       | C           | I                |
| Assess current application state       | A                   | R                    | C                   | I                       | I           | I                |
| Design Azure Spring architecture       | A                   | C                    | R                   | C                       | I           | I                |
| Set up Azure Spring environment        | A                   | C                    | R                   | R                       | I           | I                |
| Refactor applications for Azure Spring | I                   | R                    | C                   | I                       | C           | I                |
| Test applications in Azure Spring      | A                   | C                    | C                   | C                       | R           | I                |



## Question4: Describe the most likely migration approach that will suit the chosen company.





## Question5: Produce a high level schedule for the migration process.

They had a 2 month's time table for this migration.





## Question6: What are the main decision criteria for the chosen company?



With support for comprehensive monitoring and diagnostics, configuration management, continuous integration (CI) and continuous deployment (CD), life cycle management, and more, the platform offered the Bosch team PaaS benefits for deploying and managing its Spring Boot–based microservices.