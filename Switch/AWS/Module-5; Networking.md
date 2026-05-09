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
- 
