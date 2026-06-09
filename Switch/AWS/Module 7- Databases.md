# Databases

## Introduction to Databases
- AWS offers a wide range of databases services to meet various data storage and management needs.
- These services are designed to be scalable, reliable, and easy to use for businesses of all sizes.
- AWS databases services include options for relationsl databases, nonrelational databases, in-memory caches, and purpose-built services for use cases like document management.

### AWS shared responsibility model
- The AWS shared responsibility model groups services into 3 categories based on the ownership of administrative tasks.
- These categories are fully managed, managed, and unmanaged.
- Let's learn more about how these categoies are related to database services:
  1. Fully managed services:
     - For fully managed services, AWS handles nearly all operational tasks like provisioning, scaling, patching, backups, performance optimization, and security patches.
     - AWS also provides built-in monitoring and metrics.
     - Fully managed AWS database services only require customers to be responsible for designing data structures and managing access controls. 
       <img width="287" height="223" alt="image" src="https://github.com/user-attachments/assets/61de96b2-b7db-4f8c-a962-24f0605cbfb0" />

  2. Managed services:
     - With managed database service, AWS handles routine tasks like backups, patching, and hardware provisioning while customers are responsible for database configuration, query optimization and performance tuning decisions.
       <img width="277" height="225" alt="image" src="https://github.com/user-attachments/assets/ef7f68b8-5a0b-424e-93ee-190ed7b3b68a" />

  3. Unmanaged services:
     - With unmanaged databases, customers are responsible for installation, configuration, patching, maintenance tasks, database security, backups, high availability setup, and performance optimization.
     - An example of an unmanaged database in AWS would be a database management system like MySQL installed directly on an Amazon Elastic Compute Cloud (Amazon EC2) instance.
       <img width="298" height="227" alt="image" src="https://github.com/user-attachments/assets/efdc3936-aff8-47e0-999a-584a13416605" />

## Relational Database Services
- Relational databases use a rigid schema that organizes collection of data into tables with rows and columns, wher relationships exist between different tables.
- Transactional data is commonly kept in relational management system. This type of system stores data in a way such that it relates to other piece of data.
- The most common way to interact with a database is by using SQL. SQL runs on a variety of databases like MySQL, PostgreSQL, Microsoft SQL server, and many more.
- If we have an on-premise environment, we're probably running one of those databases, and they're most likely housed in our data center.
- In fact, some companies are so fond of their database deployments that they want to keep them, but they still want to move to the cloud. So, they do both. These companies can perform a lift-and-shift. This means taking their existing database deployment, lifting it up from their on-premises environment, and shifting it onto and Amazon EC2 instance.
- They still have control over the same variables they did in their on-premises environment, such as operating system, memory, CPU, storage capacity, and so forth. All they've done is moved from one location to another, with the other being the AWS cloud in this case. Here, they can use a service called AWS Database Migration Service to help make the process smoother.
- In order to make database management even more seamless, we have an AWS service called Amazon Relational Database Service or Amazon RDS, and it offers several database engines we can use.
- However, the key part here is that Aamazon RDS comes with added benefits. These benefits iinclude automated patching, backups, redundancy, failover, and disaster recovery, all of which we normally need to manage ourself. Aamzon RDS is a popular option to AWS customers because it makes it possible for us to focus on business problems instead of maintaining databases.
- By using Amazon RDS, we benefit from the AWS shared responsibility model. This means, we can take advantae of AWS's expertise in infrastructure management while maintaining the security posture of our database.
- Here, AWS manage the underlying infrastructure, operating system, database software patching, and backup automation. And, we are responsible for implementing security measures and configuring encryption for our data at rest and in transit.
- We can also migrate and deploy our database to Amazon Aurora. This is a fully managed relational database option. It supports postgreSQL, MySQL, and distributed SQL databases, or DSQLs.
- An Aurora DB cluster can include upto 15 Aurora replicas located across Availability Zones in the cluster's AWS Region. Additionally, AWS Backup supports continuous backup from Amazon Aurora. This allows specific point-in-time restoration within our retention period of upto 35 days.

### Relational databases
- 
