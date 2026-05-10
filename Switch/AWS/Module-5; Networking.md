# Networking

## Introduction to Networking
- The term networking referes to interconnected devices that can exchange data and resources.
- Networking in the AWS Cloud consists of the infrastructure and services working together to host our applications, data and any other resources we migth need.
- Amazon Virtual Private Cloud (VPCs help us provision a logically isolated section of AWS Cloud.
- In this virtual network, we can launch whatever resources we decide on. More importantly, these resources can be public or private.
- Public-facing resources have access to the internet, whereas private resources do not have internet access.

### Networking components

#### Amazon Virtual Private Cloud (Amazon VPC)
- An Amazon VPC lets us provision a logically isolated section of the AWS Cloud where we can launch AWS resources in a virtual network that we define.
 <img width="313" height="179" alt="image" src="https://github.com/user-attachments/assets/99ee7b05-d53a-4076-8b65-40b77355e81a" />

#### Subnet
- Subnets are used to organize our resources and can be made publicly or privately accessible.
- A private subnet is commonly used to contain resources like a database storing customer or transactional information.
- A public subnet is commonly used for resources like a customer-facing website.
   <img width="298" height="164" alt="image" src="https://github.com/user-attachments/assets/133da513-7b05-4074-9322-10fa839e007b" />

### Networking components: Understanding connections through diagrams
- A diagram is, simply put, a schematic or map of our network in the AWS Cloud. It can provide a visual of how users or applications access services, resources, or data.
- A picture is worth a thousand words. With a quick glance, we can see if the network was built for redundancy, security, and even scalability.
- It can also serve as a blueprint to remember important connections when building our solutions.
- Let's understand the network diagrams:
   <img width="521" height="293" alt="image" src="https://github.com/user-attachments/assets/6cd324fe-9ef8-4ece-a6a3-c77d5811ae7d" />

  1. AWS Cloud, Regions, Amazon VPC, and AZs
     - The AWS Cloud is the outermost box in most diagrams.
     - Region is the next box. AWS Regions are separate geographic areas. We choose our Region based on our user's geographic location for lower latency, compliance and data residency requirements, available services, and cost.
     - Amazon VPC is a solid box, and it represents isolated, logically segmented network within AWS. A VPC helps us to control our network resources and security.
     - Availability Zones are shwon as separate boxes across a region. AZs consist of one or more discrete data centers, each with redundant power, networking, and connectivity, and housed in separate facilities. Using multiple AZs can protect our applications from the failure of a single location in the region. 
  2. Private subnet:
     - Subnets are essentially segments our VPC, allowing us to devide our VPC into smaller, manageable sections.A subnet is a range of IP addresses in our VPC.
     - Private subnets are designed to isolate resources that shouldn't be directly exposed to the public internet. In diagram, they are illustrated with solid boxes.  
  3. Public subnets:
     - Public subnets are designed to provide direct internet access to resources placed inside them.
     - To allow access, they are connected with an internet gateway.
     - In diagram, public subnets are drawn with dashed boxes.  

## Organizing AWS Cloud Resources
- VPC (Virtual Private Cloud) is essentially our own private network in AWS. When we use a VPC, we can define our private IP range for our AWS resources and place things, like EC2 instances and elastic load balancers, inside our VPC.
- So, we don't go throwing our resources into one big VPC network space and then move on. Instead, we place them into different specific subnets.
- Subnets are chunks of IP addresses in our VPC that we can use to group resources together.
- Subnets, along with networking rules, control whether resources are either publicly or privately available.
- This idea of public compared to private access  to resources is super important. For some VPCs, we might have internet-facing resources that the public should be able to reach, like a public website or a load balancer.
- However, in other scenarios, we might have resources that we only want to be reachable if someone is logged into our private network. This might be internal services, like an HP application or backend database.
- To allow traffic from the public internet to flow into and out of our VPC, we must attach what is called an internet gateway to our VPC. An internet gateway is like a doorway that is open to the public. Without this internet gateway, non one can reach the resources placed inside of our VPC.
- Now, we want a private gateway that only allows people in if they are coming from an approved network, not the public internet. This private doorway is called a virtual private gateway, and it allows us to create a VPN connection between private network, like our on-premise data center or internal corporate network to our VPC.
- So, if we want to establish an encrypted VPN connection to our private internal AWS resources, we need to attach a virtual private gateway to our VPC.
- While, the VPN provides a secure connection, it still routes our traffic through a shared network, which can sometimes lead to slowdowns, especially when many people are using it at the same time. It's not that the VPN itself is slow of a bad option, but rather we may need higher bandwidth or a dedicated line in certain scenarios.
- So, we might want dedicated private connection to AWS. With AWS, we can achieve that using a service called AWS Direct Connect. Direct Connect lets us establish a completely private, dedicated fiber connection from uor data center to AWS. It ensures both security and consistent high performance. We work with a direct connect partner in our area to establish this connection, because, direct connect provides a physical line that connects our network to our Amazon VPC. This can help us meet regulatory and compliance needs, as well as sidestep any potential bandwidth issues.

### Organizing resources in the AWS Cloud
- We have users, which might create millions of resources using the AWS services, such as Amazon EC2 instance. Withtout boundaries around all these resources, network traffic can flow between them unrestricted.

### Establishing boundaries around AWS resources
- When organizing our resources in the AWS Cloud, we need to be able to group certain functions together and isolate them from the public, or make then available to the public.

#### Amazon VPC
- With Amazon VPC, we can provision an isolaed section of the AWS Cloud. In this isolated section, we can launch resources in a virtual network that we define.
- It provides 3 main benefits.
  1. It helps increase security because we can secure and monitor connections, screen traffic, and restrict instance access.
  2. Amazon VPC gives us full control over our resource placement, connectivity, and security.
  3. The convenience of using Amazon VPC means we will spend less time setting up, managing, and validating ourvirtual network when compared to on-premise network management.  
  <img width="307" height="117" alt="image" src="https://github.com/user-attachments/assets/1992d3a8-b4bb-40b7-aff7-4d9d907bf9b7" />

##### Subnets
- Within an Amazon VPC, we can reorganize our resources into subsections or subnets.
- A subnet is a section of an Amazon VPC that can contain resources, such as Amazon EC2 instances. 

##### Connecting our resources with an internet gateway
- To allow public traffic from the internet to access our VPC, we attach an internet gateway to the VPC.
- An internet gateway is a connection between a VPC and the internet.
- Example: We can think of an internet gateway as being similar to a doorway that customers use to enter the coffee shop.
- Without an internet gateway, no one can access the resources within our VPC.
  <img width="392" height="163" alt="image" src="https://github.com/user-attachments/assets/5960d37b-de82-4194-83de-a51dcfd74a50" />

### Virtual private gateway
-  The internet gateway (public) is open and accessible to anyone. We want a way  to protect the traffic we send on the internet from the public, internet service providers, and other who might be trying to track or interceopt it.
-  This is where virtual private network (VPN) connection comes in.
-  VPN creates a connection that is more like a secure tunnel through the internet.
-  Using encryption, it hides and protects everything we send and receive from the outside.
-  A virtual private gateway is the component in the AWS Cloud that makes it possible for us to connect this protected traffic to enter the VPC. With VPN connection, our data travels privately and safely, hidden from others using the same route.
-  With a virtual private gateway, we can establish a VPN connection between VPC and a private network, such as an on-premise data center or internal corporate network.
-  A virtual private gateway allows traffic into the VPC only if it is coming from an approved network.
   <img width="407" height="164" alt="image" src="https://github.com/user-attachments/assets/cd483369-edf7-4bf9-8672-aafc08694a92" />


- Let's understand each of the preceding networking components:
  1. Virtual Private Cloud (VPC): Amazon VPC is used to establish boundaries around our AWS resources.
  2. Virtual private gateway: A virtual private gateway allows protected internet traffic to enter into the VPC.
  3. Virtual private network (VPN) connection: A VPN encrypts our internet traffic, helping protect it from anyone who might try to intercept or monitor it. 

## More ways to connect to the AWS Cloud
### Connecting to the AWS Cloud
- With so many different types of networks, on-premises datacenters, and remote workers, companies need wide range of ways to connect to the AWS Cloud.
- So, we have 4 ways to connect to the AWS Cloud:
  1. AWS Client VPN
  2. AWS Site-to-Site VPN
  3. AWS PrivateLink
  4. AWS Direct Connect
  <img width="409" height="310" alt="image" src="https://github.com/user-attachments/assets/b054765a-214e-415d-8c82-8b3ff5c1ec9d" />

### Securely connect a remote workforce to AWS Cloud resources
- Imagine a company with a recent acquisition needing to securely connect their new remote workforce to their AWS Cloud resources.
- Even the largest companies with worldwide remote workers can quickly scale up and connect to the AWS Cloud.
- That's where AWS Client VPN can help.

#### AWS Client VPN
- AWS Client VPN is a networking service we can use to connect our remote workers and on-premise networks to the cloud.
- It is fully managed, elastic VPN service that automatically scales up or down based on user demand.
- Because, it is a cloud VPN solution, we don't need to install and manage hardware or try to estimate how many remote usrs to support at one time.
- **Benefits**: AWS Client VPN provides advanced authentication, remote access. It is elastic and fully managed.
- **Use case**: It can be used to quickly scale remote-worker access.

  <img width="333" height="251" alt="image" src="https://github.com/user-attachments/assets/00fb5538-a7dc-46bb-a3d0-34a580d3c3ff" />

- Client VPN, a managed VPN service, provides secure access to AWS resources and on-premise networks from anywhere.
- It uses an OpenVPN-based client, and it works with global regions by using the AWS global network.

### Securely connect sites to other sites
- Some companies might want to establish secure, encrypted connections between their on-premises networks like data centers or branch offices and their resources in their Amazon VPC.
- That's where Site-to-Site VPN can help.

#### AWS Site-to-Site VPN
- Site-to-Site VPN creates a secure connection between our daa center or branch offices and our AWS Cloud resources.
- **Benefits:** Site-to-Site VPN provides high availability, secure and private session, and accelerates applications.
- **Use cases:** It can be used fro application migrations and secure communication between rmeote locations.
 <img width="260" height="182" alt="image" src="https://github.com/user-attachments/assets/00094ce5-3815-4efe-a268-fa56b81962f0" />

### Securely connect resources, ven in other VPCs
- Other companies sometimes need the flexibility to privately connect to resources in other cloud providers as though they were in their own VPC.
- They need a way to communicate with these resources and don;t want the hassle of setting up gateways or site-to-site.
- That's where AWS PrivateLink can help.
#### AWS PrivateLink
- AWS PrivateLink is highly available, scalable technology that we can use to privately connect our 
