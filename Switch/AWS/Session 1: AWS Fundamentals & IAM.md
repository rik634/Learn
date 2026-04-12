#  AWS Fundamentals & IAM

## Cloud computing:
- Clous computing is the on demand delivery of IT services over the internet with pay-as-you-go pricing.
- Cloud deployment types:

  1. Clous based
  2. on primse
  3. Hybrid

- Benefits of AWS Cloud:

  1. Trade fixed expense for variable expense
  2. Benefit from massive economies of scale
  3. Stop guessing capacity
  4. Increase speed and agility
  5. Stop spending money to run and maintain the data centers
  6. Go global in minutes


## Amazon EC2:
- Amazon EC2 is more flexible, cost-effective, and faster than managing on-premise servers. It offers on-demand computer capacity that can be quickly launched, scaled and terminated, with cossts based only on the active usage.
- Compute refered to the processing power needed to run applications, manage data, and perform calcualtion.
- Compute in the cloud  means creating virtual machines with a cloud provider to run applications and tasks over the internet.
- Amazon EC2 stands for Amazon Elastic Compute Cloud.
- EC2 instances are virtual machines or VMs. VMs share the underlying physical host machine with mutiple other instances, which is a concept called multi-tenancy. In multi-tenant environment, we need to make sure that each VM is isolated from each other but is still able to share resources provided by the host. This job of resource sharing and isolation is being done by a piece of software called as hypervisor, which is running on the host machine. 
- For EC2, AWS manages the underlying host, the hypervisor, and the isolation from instance to instance. 
- When we provision an EC2 instance, we can chosse the OS, software we want to be running on our application.
- EC2 instances are also resizable. Means, if needed, we can give more CPU and memory to it. This is what we call, vertically scaling an instance. 
- We can also control the networking aspect of EC2.
- AWS services like AWS system manager offer a secure and simplified method for accessing instances.

### EC2 instance types:
- The instance types are grouped under families, which offer varying combinations of CPU, memory, storage and networking capabilities.
- The different instance families are:

  1. General purpose

     - They provide the good balance of compute, memory and networking resources.
     - They can be used for diverse workloads like web services or code repositories and when workload performance is uncertain. 
  2. Compute optimized

     - They are ideal for compute intensive tasks such as gaming servers, high performance computing, machine learning tasks, even scientific models.
  3. Memory optimized

     - They are good for memory intensive tasks such as processing large data sets, data analytice, and databases.
     - They deliver fast performance for workloads that process large datasets in memory.
  4. Storage optimized

     - They are designed for workloads that require high performance for locally stored data, such as large databases data warehousing, and I/O intensive applications.
  5. Accelerated computing

     - They are good for floating-point number calculations, graphic processing and data pattern matching.
     - They use hardware accelerators. These are co-processors that perform functions more efficiently than is possible in software running on CPUs.

### How to provision AWS resources:
- In AWS, tasks such as launching an EC2 instance, stopping an instance, or modifying instance settings are done through API requests.
- APIs provide predefined methods to interact with, manage, and configure AWS resources efficiently.
- 

### How to launch EC2 instace:
- steps are as follows:

  1. Click "Launch Instance"
  2. select AMI (Amazon Machine Image), which defined the operating system and might include additional software.
  3. select instance type, which defines the underlying hardware resources, such as CPU, memory, and network performance. 
  4. select key pair (This key pair is used to login into an EC2 instance), such as public key (whcih is going to be injected into the EC2 instance), private key (which we will keep).
  5. we can connect to EC2 instance in various ways. Users or administrators can connect using SSH for linux instances or Remote Desktop Protocol (RDP) for windows instances. 
- 
