# Storage

## Introduction to Storage
- AWS provides many solutions for storing, accessing, managing, and backing up our data in the cloud. These virtual storage systems eliminate the need for physical hardware in our data center while offering the flexibility to scale as our needs change.
- AWS offers 3 distinct types of cloud storage to meet diverse requirements and application needs:
  1. Block storage
     - Block storage breaks data into manageable pieces called blocks, which makes it fast and more efficient to access.
     - With block storage, data can be updated, block by block, meaning the whole file doesn't need to be changed  every time we make an update.
     - This makes it ideal for developers who work with applications or databases that need quick and frequent updates. 
  2. Object storage
     - Object storage saves data in self-contained units, as objects.
     - Each of these objects includes the data, a unique ID, and information about the object called metadata that makes it easy to organize and retrieve.
     - Unlike block storage, where we can update small parts of a file as needed, this type of storage requires rewriting the entire object for every change.
     - Objects are organized flat structured called buckets, which are different from traditional hierarhical systems like folders.
     - Object storage is best for files that dont' change constantly, like videos, backups, or logs.
  3. File storage 
     -File storage uses a hierarchical file system that can be shared by applications.
     It's compatible with most systems, which means little or no code modification in most cases.
     File storage is ideal for applications that require shared access, like content management systems.
     
- Beyond these categories, we need to mention databases. They allow storage of organized information that usually needs to be queried, updated, and analyzed constantly.

### Block Storage
- Block storage provides persistent, low-latency block-level storage volumes that attach to EC2 instances like physical hard drives.
- Block storage volumes can be encrypted, backed up via snapshots, and modified while in use without disrupting the instance.
- AWS offers 2 primary block storage services:
  1. **Amazon EC2 instance store:** An unmanaged non-persistent, high performance block storage directly attached to EC2 instances for temporaray data.
  2. **Amazon Elastic Block Store (EBS):** A managed service that provides persistent block storage volumes for EC2 instances, offering various types of different workloads.      

### Object storage
- Object storage is a data storage architecture that manages data as objects in a flat address space.
- It offers unlimited scalability so we can store vast amount of unstructured data without worrying about capacity constraints.
- Object storage provides enhances metadata capabilities to provide more efficient data management, searh, and analytics across massive datasets.
- The following is the primary AWS object storage service:
  1. Amazon Simple Storage Service (S3):
     - A fully managed scalable object storage service for storing and retrieving any amount of data from anywhere. 

### File storage
- AWS file storage services provide shared file systems accessible over networks, so multiple users and applications can access the same data simultaneously.
- They offer scalability and flexibility so we can expand storage capacity as needs grow without managing physical infrastructure.
- AWS offers 2 primary file storage services:
  1. Amazon Elastic File System (EFS)
     - A fully managed, scalable NFS file system for use with AWS Cloud services and on-premise resources. 
  2. Amazon FSx:
     - A fully managed file  storage services for popular file systems like Window, Lustre and NetApp ONTAP.
    
### Additional storage services
- These services don't fit cleanly into the categories we've defined do far, but they're important AWS storage offerings that we should be familiar with.
  1. AWS Storage Gateway:
     - A fully managed, hybrid-cloud storage service that provides on-premise access to virtually unlimited cloud storage 
  2. AWS Elastic Disaster Recovery:
     - A fully managed service that streamlines the recovery of our physical, virtual, and cloud-based servers into AWS. 

### AWS shared responsibility
#### AWS storage services
- The AWS shared responsibility model groups services into 3 categories based on the ownership of administrative tasks. These categories are fully managed, managed, and unmanaged.
  1. Fully managed services
     - For fully managed storage services, AWS is responsible for everything from the hardware and infrastructure up through the entire storage stack.
     - This includes data durability, availability, encryption at rest, and replication.
     - Customers are only responsible for data management, access controls, and proper service configuration.
       <img width="293" height="230" alt="image" src="https://github.com/user-attachments/assets/0fa5b4e7-35c8-4aee-9414-c02093524ba5" />

  2. Managed services:
     - For managed storage services, AWS manages the underlying storage infrastructure, hardware redundancy, and volume replication.
     - Customers are responsible for data backup strategies, encryption configuration, volume performance optimization, and capacity planning.
       <img width="283" height="223" alt="image" src="https://github.com/user-attachments/assets/6ae6a497-cda2-418e-8d6c-6cdbf031ba2f" />

  3. Unmanaged services:
     - For unmanaged storage services, customers takes full responsibility for data management, backup/recovery, encryption, performance optimization, and durability.
     - AWS only maintains the underlying physical hardware and network infrastructure.
       <img width="284" height="226" alt="image" src="https://github.com/user-attachments/assets/46d33f1a-b862-410a-be8f-33a7f4e294ca" />

## EC2 instance store and Amazon Elasic Block Store (Amazon EBS)
- 
