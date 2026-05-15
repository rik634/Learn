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
- 
