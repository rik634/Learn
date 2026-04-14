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
- There are 3 main ways we can call AWS APIs:

  1. AWS Management Console

     - It is a web interface for managing the AWS services, offering quick access to services, search functionality and simplified workflows. 
     -  Here, we can manage our resources visually.
     - It is helpful for setting up test environments, viewing AWS bills, monitoring resources, view alarms, checks billing and managing non-techincal tasks supporting multiple logged-in identities at once.
     - Good for: Users who prefer a visual, easy-to-use interface for managing and configuring AWS resources.
       
  2. AWS CLI (Command Line Interface)

     - We can use AWS CLI to invoke AWS APIs using terminal.
     - It allows us to interact with AWS services through text-based input called commands.
     - This makes automation through scripting possible.
     - Here, we can run commands using AWS CloudShell, which is a cloud based terminal that has AWS CLI already installed in managed environment.
     - command examples:
 
       1. To create EC2 instance: aws ec2 run-instances
       2. List all AZs in current region: aws ec2 describe-availability-zones
     - We can either run these commands manually or include these in script or other automation processes.
     - Automation is an important aspect to having successful and predictable deployment over time.
     - Good for: Advanced users and developers who need to automate tasks, script actions, and manage AWS resources efficiently from command line.
       
  3. AWS Software Development Kit (SDK):

     - AWS SDK cimplifies integrating AWS services into our application by providing APIs for various programming languages.
     - AWS SDK makes it possible for us to interact with AWS resources through various programming languages.
     - Good for: Developers who want integrate AWS services into their applications using language specific APIs. 
     - Example: e can run a python script using the integrated development environment such as Visual Studio Code (VS Code), that uses SDK to list the EC2 instances in the current region.
 
### Compute and shared responsibility:
- The AWS shared responsibility model outlines the division of duties between customer and AWS.
- AWS handles the security of the cloud (hardware and infrastructure), and customer handles the security in the cloud (applications, data, and access control).
- An unmanaged service like Amazon EC2 requires us to perform all the necessary security configurations and management tasks.
- When we deploy and EC2 instance, we are responsible for configuring security, managing the guest operating system (OS), applying the updates, and setting up the firewalls (security groups). 

### How to launch EC2 instace:
- steps are as follows:

  1. Click "Launch Instance"
  2. select AMI (Amazon Machine Image), which defined the operating system and might include additional software.
  3. select instance type, which defines the underlying hardware resources, such as CPU, memory, and network performance. 
  4. select key pair (This key pair is used to login into an EC2 instance), such as public key (whcih is going to be injected into the EC2 instance), private key (which we will keep).
  5. select network setting
  6. Choose storage option for our EC2 instance
  7. In the advanced section, in user-data section, we are allowed to paste a script such as below one, which will install and activate the nginx web server, which we will be using to serve content to the internet.

    <img width="507" height="234" alt="image" src="https://github.com/user-attachments/assets/fc2dded9-a310-48fd-a238-cb978035e973" />

  9. Click on "Launch instance" to launch the instance. 
- After the insatnce is launched, we can see the details of the launched instances.If we get the IP address of the instance and paste it in browser and hit enter, we can see the UI, to know, our instanc is up and running.
-  we can connect to EC2 instance in various ways. Users or administrators can connect using SSH for linux instances or Remote Desktop Protocol (RDP) for windows instances. 

### Amazon Machine Image (AMI):
- AMIs are pre-built virtual machine images, that have the basic components for what is needed to start an instance.
- AMI components:

  - An AMI includes operating system, storage setup, architecture type, permissions for launching, and any extra software that is already installed.
  - We can use one AMI to launch several EC2 instances that all have the same setup.

- 3 wasy to use AMIs:

  1. We can create our own by building a custom AMI with specific configurations and software tailored to our needs.
  2. We can use pre-configured AWS AMIs, which are set up for common operating systems and software.
  3. We can purchase AMIs from AWS marketplace, where third party vendors offer specialized software designed for specific use case.

- AMI repeatability:

  - AMIs provide repeatability through consistent environment for every new instance. Because configurations are identical and deployments are automated, development and testing environments are consistent.
  - This helps when scaling, reduces errors, streamlines managing large-scale environments.

### Amazon EC2 pricing:
- The different options fro billing for Amazon EC2 are:

  1. On-demand:

    - Here, we only pay for the duration our instance runs.
    - This can be per hour or per second based on the instance type and OS we choose.
    - Also, no long-term commitments or upfront payments are needed.
       
  2. Savings plan:

     - It offers lower EC2 prices for commitment to a consistent amount og usage.
     - This is measured in dollars per hour for a one-year or three-year term.
     - This can provide savings of upto 72 percent.
     - Also, it can lower prices on our EC2 usage, regardless of instance family, size, OS, tenancy, or AWS region. Additionally it applies to AWS Fargate adn AWS Lambda usage.
    
  3. Reserved instances:

     - These are well-suited for steady-state workloadsor ones with predictable usage.
     - They offer upto 75% discount compared to on-demand pricing. We qualify for the discount after we commit to 1 one-year or 3 year term.
     - It also has 3 payment options:
 
       1. All upfront: Where we pay for them in full when we commit
       2. partial upfront: Where we pay for a portion when we commit
       3. No upfront: Where we don't pay anything at the beginning 
       
  4. Spot instances:

     - Here, we can request spare EC2 capacity for upto 90% off of the on-demand price. The catch here is that AWS can reclaim the instance at any time.
     - But, we do receive a 2 minute warning , so we can save our progress. And we can always resume later if we needed.
     - So, if we are choosing spot instances, we need to make sure that our workload can tolerate being interrupted.
       
  5. Dedicated hosts:

     - These are physical servers that customers can reserve for exclusive use.
     - No other customer's workload can share the server. This isolation use case is ideal for security-sensitive or licensing-specific workloads like windows or SQL server.
     - Here,w e have control over instance placement or resource allocation.
       
  6. Dedicated instance:

     - Here, we pay for instances running on hardware dedicated solely to our account.
     - This option provides isolation from other AWS customers.
 
- Dedicated host vs. dedicated instances:

  - Dedicated Hosts provide exclusive use of physical servers, offering full control over instance placement and resource allocation. This makes them ideal for security- or compliance-driven workloads. But what if you don’t need that level of control?
  - You could use Dedicated Instances, which offer physical isolation from other AWS accounts while still benefiting from the flexibility and cost savings of shared infrastructure.
  - The key difference is that Dedicated Instances provide isolation without you choosing which physical server they run on. Dedicated Hosts give you an entire physical server for exclusive use, providing complete control over instance placement and resource allocation.
  - Dedicated Hosts offer exclusive use of a server with full control, whereas Dedicated Instances provide isolation without server control.

### Scaling Amazon EC2:
- If you've ever tried to access a website that wouldn't load and kept timing out, it might have been overwhelmed by more requests than it could handle. Here, scalability  helps us manage fluctuating demand by adjusting compute capacity.
- Scalability is about a system’s potential to grow over time, whereas elasticity is about the dynamic, on-demand adjustment of resources.

  1. Scalability:

     - Scalability refers to the ability of a system to handle an increased load by adding resources.
     - You can scale up by adding more power to existing machines, or you can scale out by adding more machines.
     - Scalability focuses on long-term capacity planning to make sure that the system can grow and accommodate more users or workloads as needed.
  2. Elasticity:

     - Elasticity is the ability to automatically scale resources up or down in response to real-time demand.
     - A system can then rapidly adjust its resources, scaling out during periods of high demand and scaling in when the demand decreases.
     - Elasticity provides cost efficiency and optimal resource usage at any given moment.

- Amazon EC2 Auto-scaling adds instances based on demand and key scaling metrics and then decommission instances when that demand goes down. Here, we also need help of other AWS services, such as AWS CloudWatch service to collect and monitor these metrics. This data is then used to determine when when scaling needs to happen. And, it happnes automatically right when we need it.

#### Amzon EC2 Auto-scaling:
- Amazon EC2 Auto Scaling automatically adjusts the number of EC2 instances based on changes in application demand, providing better availability. It offers two approaches.

  1. Dynamic scaling adjusts in real time to fluctuations in demand.
  2. Predictive scaling preemptively schedules the right number of instances based on anticipated demand.

- With EC2 Auto Scaling, you maintain the desired amount of compute capacity for your application by dynamically adjusting the number of EC2 instances based on demand. You can create Auto Scaling groups, which are collections of EC2 instances that can scale in or out to meet your application’s needs.
- An Auto Scaling group is configured with the following three key settings.

  1. Minimum capacity:

     - The minimum capacity defines the least number of EC2 instances required to keep the application running. This makes sure that the system never scales below this threshold. It's the number of EC2 instances that launch immediately after you have created the Auto Scaling group. 
  2. Desired capacity:

     - The desired capacity is the ideal number of instances needed to handle the current workload, which Auto Scaling aims to maintain. If you do not specify the desired number of EC2 instances in an Auto Scaling group, the desired capacity defaults to your minimum capacity.
  3. Maximum capacity:

     - The maximum capacity sets an upper limit on the number of instances that can be launched, preventing over-scaling and controlling costs. For example, you might configure the Auto Scaling group to scale out in response to increased demand.

- Because Amazon EC2 Auto Scaling uses EC2 instances, you pay for only the instances you use, when you use them. This gives you a cost-effective architecture that provides the best customer experience while reducing expenses.

### Directing traffic with elastic load balancing:
- 
