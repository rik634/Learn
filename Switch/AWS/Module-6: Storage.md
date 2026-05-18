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
- AWS offers 2 types of block storage: Amazon EC2 instance store and Amazon Elastic Block Store or Amazon EBS.

- When we're using Amazon EC2 to run our business applications, those applications neeed access to CPU, memory, network, and storage. EC2 instances give use access to all of those components.
- As applications run, they will oftentimes need access to block-level storage. We can think of block-level storage as a place to store files.
- A file being a series of bytes that are stored in blocks on disk. When a file is updated, the whole series of blocks aren't all overwritten. Instead, it updates just the pieces that need to be changed. This makes it an efficient storage type when working with applications like databases, enterprise software, or file systems.
- When we use our laptop, we are accessing block-level storage. Here, we are talking about the hard-drive.
- EC2 instances have hard-drives too, and there are few different types.
- Depending on the type of the EC2 instance we launch, it can provide us with physically attached local storage called instance store volume. The catch here is that because this volume is attached to the underlying physical host, if we stop or terminate our EC2 instance, all of data written to the instance store volums will be deleted.
- The reason for this is that if we start our instance from a stopped state, it's likely that EC2 instance will start up on another host. A host where that volume does not exist. Because of the ephemeral or temporary nature of instance store volume, they are useful in situations where we can afford to lose the data being written to the drive. Like if we need scratch space for heavy calculations or data processing. 
- With Amazon EBS, we can create virtual hard drives, which we call EBS volumes, that we can attach to our EC2 instances. These are separate drives from the local instance store volumes, and they aren't tied directly to host that our EC2 instance is running on. This means, that the data that we write to an EBS volums can persist between stops and starts of an EC2 instance.
- EBS volumes come in all different sizes and types. How this works is we define the size, type, and configurations of the volume we need. Provision the volums, and then attach it to our EC2 instance. From there, we can configure our application to write to the volums, and we're good to go.
- If we stop and then start the EC2 instance, the data in the volume remains. EBS offers different volume types that gives use different levels of performance and offer different pricing. Performance for EBS volumes is measured in IOPs, or Input/output per second.

### Amazon EC2 instance store
- Amazon EC2 instance store isn't a stand-alone AWS block storage service. Rather, it refers to the block-level storage that is physically attached to the EC2 instance host computer.
- Depending on the type of instance, EC2 instance store might come attached as the default storage.
- Since its data is lost when an instance is stopped or terminated, EC2 instance store is best for temporary memory-based storage needs liek buffers, caches and scratch data.
- It is not recommended for applications that require data retention.

#### Key takeway: no data persistence
- An Amazon EC2 instance store provides temporary block-level storage for an Amazon EC2 instance.
- This means that if we stop or terminate an Amazon EC2 instance, all the data written to the attached instance store is deleted.
- Let's learn how Amazon EC2 instance store manages data when an EC2 instance is stopped:
  1. An EC2 instance is running with data being stored in an EC2 instance store
  2. The EC2 instance is stopped
  3. After the EC2 instance is stopped or terminated, all data within the EC2 instance store for that instance is lost.
 
#### Benefits
1. Automatically available storage:
  - Instance store volumes come automatically attached to many EC2 iinstance types, providing temporary block-level storage at no additional cost.
  - It's physically connected to the host computer, offering high I/O performance for data that disappears when the instance stops. 

2. Cost effective:
   - Because EC2 instance store is included in the EC2 instance price, we don't have to pay any additional fees for storage.
   - It's ideal for temporary storage needs like buffers, caches, or scratch data, potentially reducing expenses for applications that don't require persistent storage.

3. High performance:
   - EC2 instance store offers extremely low-latency storage directly attached to the host server of our EC2 instance.
   - This proximity means exceptionally high I/O performance, making it ideal for temporary storage of data requiring fast processing.

### Amazon Elastic Block Store (EBS)
- Amazon EBS provides persistent block-level storage volumes for use with Amazon EC2 instances.
- EBS volumes act like external hard-drives, offering consistent and low-latency performance for workloads like databases and file systems.
- EBS volumes can be conveniently backed up, resized, and attached to different EC2 instances. To create an EBS volume, we define the configuration for things like volume size and type.
- After the volume has been created, it can be attached to an Amazon EC2 instance. Because EBS volumes are for data that needs to persist, it's important to back up the data. It's recommended that we take incremental backups of EBS volumes by creating Amazon EBS snapshots.

#### Key takeaway: data persistence
- Amazon EBS provides block-level storage volumes that we can use with Amazon EC2 instances. If we stop or terminate an Amazon EC2 instance, all the data on the attached EBS volume remains available.
- Let's learn how Amazon EBS manages data when an EC2 instance is stopped:
  1. An EC2 instance is running with data being stored in an attached EBS volume.
  2. The EC2 instance is stopped.
  3. After the EC2 instance is stopped or terminated, all data stored within the EBS volume is retained. 

#### Use cases:
- Some practical use cases for Amazon EBS include daatbase hosting, backup storage for applications, and rapid deployment of development environments using volume snapshots.

#### Benefits:
- EBS volumes support data portability through their ability to detach and reattach to instances as needed. There are many practical reasons we might choose to do this.
  1. Data migration: 
     - EBS volumes can be easily migrated between Availability Zones using snapshots. The snapshots provide a simple way to move data across regions or create copies. 
  2. Instance type changes:
     - Since EBS volumes remain independent of EC2 instances, it's not complicated to attach them to different instance types.
     - This flexibility lets us upgrade or downgrade instances without losing data. 
  3. Disaster recovery:
     - EBS snapshots provide reliable backup solutions that can be restored in different regions during emergencies.
     - Regular automated snapshots ensure our data remains protected and quickly recoverable. 
  4. Cost optimization:
     - EBS volumes can be modified to different types and sizes to match actual usage patterns.
     - We can switch between storage types and adjust capacity without downtime. 
  5. Performance tuning:
     - Amazon EBS offers various volume types to match different workload requirements and IOPS needs.
     - We can adjust volume performance characteristics on the fly to meet changing application demands. 

## Amazon Elastic Block Store (Amazon EBS) Data Lifecycle
-  Amazon EBS data lifecycle management involves creating, backing up, and deleting volumes and snapshots.
-  This process optimizes storage costs and helps to ensure data protection.
-  As we know from the shared responsibility model, we are responsible for managing our own data. This means, we need to manage our EBS volumes lifecycle, which includes provisioning, moving, deprovisioning, and backups.
-  This is where Amazon EBS snapshots come in. These are point-in-time copies of our EBS volumes. Point-in-time means, that the backup is done at a specific moment- in time. This can be daily, weekly, or even monthly depending on our organization's needs. Also, this means, we can make incremental backups and restore them as needed.
-  What does incremental backup means? : Let's say, we take the first snapshot of our daat on monday. All of our data is copied in its entirely. Then, on Tuesday, we take another snapshot, but this time, it only copies what has changed since the previous snapshot was taken. That's the incremental part.
-  This approach makes subsequent snapshots faster and more-storage efficient, which translates to significant reduction in cost compared to running in full backup every time.
-  Logging into the AWS Management Console every week and create a snapshot is time consuming and prone to human error. Here, in order to automate this, we can use fully-managed service like Amazon Data Lifecycle Mananger.
-  Wuth Amazon Data Lifecycle Manager, we can define policies to help automate snapshot lifecycle management. We can schedule snapshot creation, set retention policies, manage lifecycles, and apply consistent backup policies across our organization.

### EBS Snapshots
- EBS snapshots ar point-in-time backups of EBS volume. They can be used for disaster recovery, data migration, volume resizing, and for creating consistent backups of production workloads.
- EBS snapshots are incremental, so they only save the blocks on the volume that have changed after our most recent snapshot.
- EBS snapshots can be used to create multiple new volumes, and new volumes created from a snapshot are an exact copy of the original volume at the time the snapshot was taken.
- Snapshot of EBS volumes are stored redundantly in multiple availability zones using Amazon S3.

#### Working with EBS snapshots
- In keeping with the AWS shared responsibility model, as the customer, we are responsible for scheduling and managing regular EBS snapshots as part of our backup strategy.
- This includes monitoring snapshot costs and deleting unnecessary snapshots to avoid excessive charges.
- We also need to make sure sensitive data within snapshots is encrypted, verify snapshot integrity, and test restoration procedures regularly.
- The following illustration shows how EBS snapshots are created from EBS volume over time.
   <img width="424" height="193" alt="image" src="https://github.com/user-attachments/assets/b2d1ba13-13cf-48b7-8c3d-5cd7c76852a9" />

- Let's learn more about how EBS snapshots interact with EBS volumes:
  1. Initial snapshot:
     - When we create the first snapshot of an EBS volume, a full copy of all the data on the volume at that point in time is included.
     - This initial snapshot serves as the baseline and contains all the data blocks that were in use on the volume. 
  2. Subsequent incremental snapshots:
     - For all snapshots after the initial one, only the blocks that have changed since the last snapshot are captured and stored.
     - These are called incremental snapshots. They're much smaller and faster to create then full snapshots.
     - Each incremental snapshot contains references to the previous snapshots, creating a chain that allows for point-int-time recovery. 
  3. Snapshot consolidation and management:
     - Despite being incremental, each snapshot appears as a full point-in-time copy of our volume.
     - The relationship between multiple snapshots of the same volume are managed automatically.
     - When we delete a snapshot, only the data unique to that snapshot is removed. Data referenced by other snapshots is preserved.
    
#### Benefits
- Below shows the main benefits of EBS snapshots and how it improves our data protection strategy in Amazon EBS:
  1. Data protection and recovery:
     - Snapshots enable fast data recovery from corruption, accidental deletion, or system failures using point-in-time backups.
  2. Operational flexibility:
     - Snapshots enable operations like cross-region data migration, volume resizing, volume cloning, and sharing data across AWS accounts. 
  3. Cost effective:
     - Snapshots use incremental backup technology, storing only changed blocks after the initial backup, reducing storage costs and backup time.

### Amazon Data Lifecycle Manager
- We can automate the creation, retention, and deletion of EBS snapshots using Amazon Data Lifecycle Manager. Amazon Data Lifecycle Manager can schedule snapshots during off-peak hours to minimize performance impact and automatically delete outdated backups to control storage costs. It's particularly valuable for large-scale deployments where manual snapshot management would be time-consuming and error-prone.
- Let's learn how you can use Data Lifecycle Manager to create custom EBS Snapshots policies:
  - By reducing manual effort and establishing consistent backup policies, Amazon Data Lifecycle Manager helps maintain compliance requirements by scheduling regular backups and enforcing retention rules.
  1. Create an EBS snapshots policy: Create an EBS snapshots policy using the Amazon EC2 console, API calls, AWS Command Line Interface (AWS CLI), SDKs, or AWS CloudFormation.
  2. Select target resource type: Choose either an EBS volume or an EC2 instance as the target for the snapshot.
  3. Exclude volumes: Narrow down the data to be included in the snapshot by choosing options to exclude either the root volume or data volumes
  4. Set custom schedules: Automate the creation, retention, and deletion of EBS snapshots by setting up custom schedules.
  5. Apply additional actions: Before finalizing the policy, we can apply additional actions. These include configuring elements of the snapshots like tags, snapshot archiving, Amazon EBS fast snapshot restore, cross-Region copying, and cross-account sharing.

- 
