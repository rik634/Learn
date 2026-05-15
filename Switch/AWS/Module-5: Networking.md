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
- AWS PrivateLink is highly available, scalable technology that we can use to privately connect our VPC to services and resources as if they were in our VPC.
- We do not need to use an internet gateway, NAT device, public IP address, Diret Connect connection, or AWS Site-to-Site VPN connection to allow communication with AWS services or resources from our private subnets.
- Instead, we control the specific API endpoints, sites, services, and resources that are reachable from our VPC.
- **Benefits:** AWS PrivateLink helps use secure our traffic and connect with simplified management rules.
- **Use case:** It is used for connecting our clients in our VPC to resources, other VPCs, and endpoints.
- Even though the preceding connections are highly available and scalable, traffic jams are possible because we're using the same connection as other clients. This is the reason, we might need a dedicated private connection with a lot og bandwidth.

### Dedicated private connections for increased bandwidth
#### AWS Direct Connect
- Direct Connect is a servicr that makes it possible for us to establish a dedicated private connection between our network and VPC in the AWS Cloud.
- **Benefits:** AWS Direct Connect reduces network costs and increases amoutn of bandwidth.
 <img width="380" height="173" alt="image" src="https://github.com/user-attachments/assets/3c7cab7c-7ffc-4691-8311-ca0f97fdce2e" />

-  Let's understand about direct connect:
   1. Latency-sensitive applications:
      - Direct Connect bypasses the internet and provides a consistent, low-latency network experience.
      - This makes it ideal for applications like video streaming and other real-time applications that require high performance. 
   2. Large-scale data migration or transfer:
      - Direct Connect helps ensure smooth and reliable data transfers at massive scale for rea;-time analysis, rapid data backup, or broadcast media processing. 
   3. Hybrid cloud architecture:
      - We can use Direct Connect to link our AWS and on-premise networks to build applications that span environments without compromising performance. 

### Additional gateway services:
- There are several different types of gateways we can use to connect our AWS resources.
- Let's understand about those gateways:
  1. AWS Transit Gateway:
     - AWS Transit Gateway is used to connect our Amazon VPCs and on-promise networks through a central hub.
     - As our cloud infrastructure expands globally, inter-Region peering connects transit gateways together using the AWS Global infrastructure. 
  2. Network Address Translation (NAT) Gateway:
     - A NAT gateway is a NAT service. We can use a NAT gateway so that instances in a private subnet can connect to services outside our VPC but external services can't initiate a connection with those instances. 
  3. Amazon API Gateway:
     - An API defines how different software systems can interact and communicate with each other.
     - The Amazon API Gateway is an AWS service for creating, publishing, maintaining, monitoring, and securing APIs at any scale. 
------------------------------------
- Let's review the 4 types of connectivity options/services that help us connect our AWS Cloud to our clients, datacenters and sites:
  1. AWS Direct Connect: AWS Direct Connect is a private, dedicated AWS connection to our data center or office.
  2. AWS Client VPN: AWS Client VPN connects our remote workforce to AWS or on-premises with a VPN.
  3. AWS Site-to-Site VPN: AWS Site-to-Site is an encrypted network connection to our Amazon VPCs.
  4. AWS PrivateLink: AWS PrivateLink connects our VPC privately to services and resources as though they were in our VPC. 

## Subnets, Security Groups, and Network Access Control Lists
- AWS has wide range of tools that cover every layer of security: Firewalls, distributed denial-of-service, or DDoS prevention, encryption, and much more.
- One of the main reasons to use subnets in a VPC is to control access to the gateways. The public subnets have access to the internet gateway, the private subnets do not. But subnets can also control traffic permissions.
- Packets are messages from internet, and every packet that crosses the subnet boundaries gets checked against something called a network access control list, or network ACL. This check is to see if the packet has permissions to either leave or enter the subnet, based on who it was sent from and how it's trying to communicate.
- We can think of network ACLs as passport control officers. If we're on the approved list, we get through. If we're not on the list, or if we're explicitly on the do-not-enter list, then we get blocked.
- Network ACLs check traffic going into and leaving a subnet. The list gets checked on our way in and on the way out. And just because traffic is let in doesn't necessarily mean they're gonna let responses back out. Only explicitly approved traffic can be sent on its way.
- A network ACL only gets to evaluate a packet if it crosses a subnet boundary, in or out. It doesn't evaluate if a packet can reach a specific EC2 instance or not. Sometimes, we will have multiple EC2 instances in the same subnet, but they might have different rules around who can send them messages and what port those messages are allowed to be sent to. So, we need instance-level network security as well.
- To solve instance-level access questions, we introduce security groups. Every EC2 instance, when it's launched, automatically comec with a security group. And by default, the security group does not allow any traffic into the instance at all. All ports are blocked. That's very secure, but perhaps not very useful if we want an instance to actually accept traffic from outside, like a message from the frontend instance or message from the internet. So, we can modify the security group to accept a specific type of traffic.
- In case of a website, we want web-based traffic like HTTPS to be accepted but not other types of traffic. If network ACLs are a passport control, a security group is like doorman at our building, the building being the EC2 instance. With security groups, we allow specific traffic in, and by default, all traffic is alllowed out.
- The key difference between a security group and network ACL is the security group is stateful. That means, it has some kind of memory when it comes to who to allow in or out. And the network ACL is stateless, which remembers nothing and checks every single packet that crosses its border regardless of any circumstances.
- By default, all outbound traffic is allowed from a security group. The network ACL dosn't care about what the security group allowed. It has ots own list of who can pass and who can't. If the traffic address is allowed, we can keep going on our journey, which it is. A network ACL has stateless control. Stateless contro means, it always checks its list.
- Network ACLs have rules for both incoming and outgoing and they can be different.
- Here, we might think about all the network overhead this might generate. The reality is all of these exchanges happen instantly as part of how AWS networking actually works.
- Security is a critical consideration in all networking for modern architectures.

### Subnets
- A section of a VPC for grouping resources based on security or operationsl needs.
 <img width="305" height="356" alt="image" src="https://github.com/user-attachments/assets/e80d0624-d0b0-4a83-a45b-4cbbea7cfe76" />

- A subnet is a section of a VPC in which we can group resources based on security or operationsl needs. Subnets can be public or private.
- **Public subnets** contain resources that need to be accessible by the public, such as an online store's website
- **Private Subnets** contains resources that should be accessible only through our private network, such as database that contains customer's personal information and order histories.
- In a VPC, we can define rules to allow resources in different subnets to communicate with each other. For example, we might have an application that uses Amazon EC2 instances in public subnet communicating with databases that are located in a private subnet.

### Network traffic in a VPC
- The movement of data packets traveling across a network.
- When a customer requests data from an application hosted in the AWS Cloud, this request is sent as a packet. A packet is a unit of data sent over the internet or a network.
- It enters into a VPC through an internet gateway. Before a packet can enter into a subent or exit from a subnet, it will run into several checks for permissions, one being a network ACL associated with the subnet the packet is being routed to. The permissions defined by the network ACLs indicate what is allowed or denied. It is based on who sent the packet and how the packet is trying to communicate with the resources in the subnet.
<img width="380" height="116" alt="image" src="https://github.com/user-attachments/assets/7b56c7e3-7ba5-4270-9d29-4ff65f82a624" />

- The VPC component that checks packet permissions for subnets is a network ACL.

### Network ACLs
- Virtual firewall controlling traffic.
- A network ACL is a virtual firewall that controls inbound and outbound traffic at subnet level.
- A network ACL checks permissions every time a packet travels across a subnet boundary.
- Each AWS  account includes a default network ACL. When configuring our VPC, we can use our account's default network ACL or create custom network ACLs. By default, our account; default network ACL allows all inbound and outbound traffic, but we can modify it by adding our own rules.
<img width="407" height="173" alt="image" src="https://github.com/user-attachments/assets/e9b9892a-4697-47de-8c78-abf8cf0d0807" />

- For custom network ACLs, all inbound and outbound traffic is denied until we add rules to specify which traffic to allow. Additionally, all network ACLs have an explicit  deny rule. This rules makes sure that is a packet doesn't match any of the other rules on the list, the packet is denied.
 <img width="406" height="179" alt="image" src="https://github.com/user-attachments/assets/af7fb97d-d6ef-488c-b991-7e0eb503f43d" />

#### Stateless packet filtering
- Network ACLs perform stateless packet filtering. They remember nothing and check packsts that cross the subnet border each way: inbound and outbound.
- This is similar to sending a request out from an Amazon EC2 instance and to the internet.
<img width="410" height="167" alt="image" src="https://github.com/user-attachments/assets/b26fc076-686a-4dac-8355-68bbb7b2da6c" />

- When a packet response for that requets comes back to the subnet, the network ACL does not remember our previous request. The network ACL checks the packet response against its list of rules to determine whether to allow or deny.

### Security groups
- Control inbound and outbound traffic at resource level. 
- After a packet has entered a subnet, it must have its permissions evaluated for resources within the subnet, such as Amazon EC2 instances.
- A security group is the VPC component that checks packet permissions for an Amazon EC2 instance. It is a virtual firewall that controls inbound and outboound traffic for specific AWS resources, like Amazon EC2 instances.
- By default, a security group denies all inbound traffic and allows all outboud traffic.
<img width="417" height="238" alt="image" src="https://github.com/user-attachments/assets/f5c609e7-0fb2-4374-9ab4-0f222cfab154" />

- With security groups, we can add custom rules to configure which traffic should be allowed. Any other traffic would then be denied. For example, custom rules can be given separately for inbound and outbound traffic.
<img width="410" height="250" alt="image" src="https://github.com/user-attachments/assets/6c0f7618-0ae5-411f-b194-aa432b562443" />

- **Note:** If we have multiple Amazon EC2 instances within the same VPC, we can associate them with the same security group or use different security groups for each instance.

#### Stateful packet filtering
-  Security groups perform stateful packet filtering. They remember previous decisions made for incming packets.
------------------------------------------------------------------------

- With both network ACLs and security groups, we can configure custom rules for the traffic in our VPC.
- Difference between security group and network ACLs:
<img width="389" height="312" alt="image" src="https://github.com/user-attachments/assets/8410a79a-1a73-4fee-9c37-3716f809d55a" />

- When it comes to securing the subnets and resources in our VPC with the network ACLs and security groups, it is our responsibility. These components make up networking traffic protection and are critical defenses in protecting our application in the cloud

## Amazon VPC demo
<img width="912" height="470" alt="image" src="https://github.com/user-attachments/assets/fed84b9c-e481-4398-a1d9-02e63ff8875f" />

- Task: Create VPC, private and public subnets, internet gateway, route table for public subnets.
  <img width="925" height="485" alt="image" src="https://github.com/user-attachments/assets/3d28f874-1ae0-4716-bb2b-33ccaaf04e53" />

- Steps:
  1. create VPC:
     - select VPC from AWS Management console.
     - create New VPC.
     - Resources to create: select VPC only
     - Name tag: Add the name for VPC
     - IPv4 CIDR block: select IPv4 CIDR manual input
       - CIDR stands for classless inter-doman routing address. This address is used to define the block of private IO addresses available to assign to any resources launched into the VPC.
     - IPv4 CIDR: add some value like (10.0.0.0/16)
     - click on "Create VPC", and VPC will get created with provided details.
     - So, with this, every resources in our VPC will get a private IP address that starts with 10.0, then second 2 numbers will vary from resource to resource, each one hacing a unique address in the VPC. 
  2. Create subnet:
     - Here, we will create both public and private subnets across 2 different availability zones or AZs.
     - This is the best practice for achieving high availability for our applications. it allow our instances to remain accessible even if one AZ experiences an ouage, by distributing them across separate physical locations within same AWS Region.
     - So, subnet is a range of IP addresses in our VPC. We can launch AWS resources into specified subnet. And we can use public subnet for resources that must be connected to internet, and a private subnet for resourced that won't be connected to ineternet.
       1. Create private subnet:
          - In the left panel for VPC dashboard, select subnet.
          - select create subnet.
          - On create subnet page, for VPC ID, select our created VPC from the dropdown list.
          - now, in subnet settings, add the fields subnet name, select desired AZ, IPv4 VPC CIDR block (will be same as we added while creatiion VPC, i.e., 10.0.0.0/16), IPv4 subnet CIDR block (we need to find CIDAR block for subnet, i.e., 10.0.0.0/24) etc.
          - select "Create subnet' and subnet will get created under specified VPC.
          - Now, select just created subnet, select actions, and select settings. Here, in edit subnet setting page, there is setting for Auto-assign IP setting. We have to make sure that this is not enabled, because enabling this setting would give every resource in the subnet a public IP. This is private subnet, so we don't want that.
          - select cancel.
          - We can create another subnet in different AZ, by following the same steps mentioned above.
       2. Create public subnet:
          - Go to VPC dashboard, and in left panel, click on subnets.
          - Click on "Create subnet".
          - On create subnet page, for VPC ID, select the VPC from dropdown.
          - Enter the subnet name, select AZ, and add IPv4 subnet CIDAR block, like (10.0.3.0/24)
          - Click on "create subnet", and subnet will be created.
          - on subnet dashbaord, select the public subnet we created just now, click on action, click on edit subnet settings.
          - On edit subnet settings page, There is Auto-assign IP settings, enable the auto-assign public IPv4 address. This setting provides a public IPv4 address for all instances or resources launched into this public subnet.
          - Click on "Save".
          - We can create another public subnet in different AZ, by following the same steps mentioned above. 
     - So, we have created 2 public and 2 private subnets. Also, even through our subnets are labeled public, they are not yet public subnets. First, the VPC needs an attached internet gateway, which we will attach in next steps. Then, we need to create appropriate routes in the route tables. 
  3.  Create internet gateway:
      - On VPC dashboard, on left pannel, click on internet gateways.
      - Click on "Create internet gateway".
      - On the create internet gateway page, add name tag for our gateway.
      - Click "create internet gateway" and the internet gateway will get created.
      - nOW, select the just now created internet gateway, select actions, select "Attach to VPC".
      - On attach to VPC page, in VPC section, select the VPC from the dropdown, to which we want to attach our internet gateway.
      - Click on "Attach internet gateway".
      - Now, the internet gateway is attached to our VPC.
      - Eeven though, we have created an internet gateway, and it is attached to VPC, we still have to tell instances within the public subnet, how to get to the internet. That's where the route table comes in.
      - A route table contains a set of rules, called routes, that are used to determine where the network traffic is directed.
      - Each subnet in our VPC must be associated with a route table. The table controls the routing for that subnet.  
  4.  Set up route table:
      - On VPC dashboard, in left panel, select "route tables".
      - We need to create a route table to route public traffic to the internet gateway.
      - Now, click on "create route".
      - On create route table page, add name, for VPC, select the VPC from the dropdown.
      - Click on "create route table", and the route table will get created.
      - Now that the route table is created, clik on the just now created route table, under the routes tab, there is one route in our route table that allows traffic within the VPC to flow within the network, but it does not route traffic outside of the network.
      - So, we need to add a new route to enable public traffic.
      - For this, click on edit routes. Click on "add route". And for this route, under the destinatino, enter (0.0.0.0/0) and for target, select the internet gateway, we created and attached to VPC.
      - Click on "save changes".
      - Now, we need to associate this route table with our public subnets. On route table page, click on subnet associations tab. In the explicit subnet association section, select edit subnet associations. On edit subnet associations page, select the public subnets we created, and click on "save associations".
      -  So, our subnets (which we selected) is now public because it is connected to the internetthrough the internet gateway.
      - But, the private subnets do not have that route, so they remain private. 
     
- security groups also use CIDR blocks to define sources or destinations for network traffic. This way, everything is secure and we can decide who can access what resources based on our needs.

### Building an Amazon VPC in the AWS Cloud:
- Core components covered in this demonstration. 
- The following is a high-level overview of the resources and core components created in the preceding demonstration.

#### Create the Amazon VPC
- Before we can create resources in the AWS Cloud, the first step is to create our own Amazon VPC. We will also specify the Region best located for our resources.
<img width="384" height="232" alt="image" src="https://github.com/user-attachments/assets/0ad6c7be-427b-450f-838f-ab6381f3a6d2" />

#### Create the subnets
- We will create 2 public and private subnets across 2 AZs. This is a best practice to achieve high availability.
<img width="382" height="229" alt="image" src="https://github.com/user-attachments/assets/03697c47-cf61-4605-9173-ce1b3714c529" />

#### Create an internet gateway and route traffic
- Withoout an internet gateway. our users can't get to our resources. First, we create the internet gateway. Then, we create route tables to route traffic to allow internet traffic in and local traffic out.
<img width="404" height="227" alt="image" src="https://github.com/user-attachments/assets/f2d4497d-d799-4c4a-a21f-02b0cf391843" />

## Global networking
- Let's say, we are hosting an application on AWS Cloud. When our users type the website's address into their browser, the site lights up. Here, 2 of the AWS services help with this, i.e., Amazon Route 53, Amazon CloudFront
- Route 53 is a domain name service, or DNS. DNS acts as a translation service. However, instead of translating between languages, DNS translates  website names into Internet Protocol, or IP addresses. Humans can read website names and computers can read IP addresses. This acts as the bridge between the two. Additionally, Route 53 can route traffic to different endpoints using several different routing policies. These include latency-based routing, geolocation, geoproximity, and weighted round robin. We can even use Route 53 to register domain names. Pick a domain name, check if it's available, and buy it using Route 53.
- Amazon CloudFront: As we know, edge locations serve content as close to customers as possible, and one part of that is a content delivery network or CDN. So, using CloudFront, we can cache our content near to the customer location. 

### Edge networking services
####Secure and speedy networking for user-facing application data
- Edge networking is the process of brining information storage and computing abilities closer to the devices that produce that information and the user who consume it.
- Edge computing is important because organizations often need lower latency access to their data and content. By performing tasks or caching data locally or closer to users, organizations can deliver faster, more responsive experiences while maintaining better control over their infrastructure.
- There are also many different services that are hosted on the edge, like DNS service, Amazon Route 53.

#### Translating domain names to IP addresses with DNS
- Suppose, a company has website hosted in the AWS Cloud. Customer can enter the web address into their browser and they are able to access the website.
- This happens because of DNS resolution. DNS resolution involves a customer DNS resolver communicating with company DNS server.
- DNS resolution is the process of translating a domain name to IP address.
- Let's understand the steps of how DNS works:
  1. When we enter the domain name into our browser, this request is sent to a customer DNS resolver.
  2. The customer DNS resolver asks the company DNS server for the IP address that corresponds to company's website.
  3. Company DNS server responds by providing the IP address for company's website, like 192.0.2.2

### Amazon Route 53
- Route 53 is a DNS that provides a reliable and cost-effective way to route users to internet applications.
- Route 53 directs end users to our resources with globally dispersed DNS servers and automatic scaling. It gives developers and businesses a reliable way to route end users to internet applications hosted in AWS.
- It connects user requests to infrastructure running in AWS, such as Amazon EC2 instances and load balancers. It also routes users to infrastructure outside of AWS.
- Another feature of Route 53 is the ability to manage the DNS records for domain names. We can register new domain names directly in Route 53. We can also transfer DNS records for existing domain names managed by other domain registrars. This makes it possible for us to manage all of our domain names withing a single location.
- Route 53 also works with the AWS edge networking server, Amazon CloudFront.

### Amazon CloudFront
- CloudFront is a content delivery network (CDN) service that delivers our content with faster loading times, cost savings, and reliability.
- CloudFront is like global network of delivery trucks that quickly brings web content to users around the world. Instead of all requests traveling back to one central warehouse (our original server), CloudFront stores sopies of our content at locations closer to our users.
- This means websites, videos, images, and applications load much faster, no matter where our customers are located.
- Use cases:
  1. Streaming video service:
     - A company that offers online workout videos uses CloudFront to make sure videos play smoothly without buffering, even during peak exercise times when thousands of users log in simultaneously. 
  2. Ecommerce website:
     - An online store uses CloudFront to deliver product images and web pages quickly during busy shopping seasons. This faster experience keeps customers engaged and reduces abandoned shopping carts. 
  3. Mobile app:
     - A travel app uses CloudFront to deliver map data and images to user;s phones quickly to help travelers navigate new cities without frustating delays. 

- The below example describes how Route 53 and CloudFront work together to deliver content to customers.
<img width="403" height="178" alt="image" src="https://github.com/user-attachments/assets/6f8f781e-cf3a-42db-8a18-46a6536201a8" />

  1. Client: A customer requests data from the application by going to company's website
  2. Amazon Route 53: Amazon Route 53 uses DNS resolution to identify "company.com"'s corresponding IP address, 192.0.2.0. This information is sent back to the customer.
  3. CloudFront: The customer's request is sent to the nearest edge location through CloudFront.
  4. Application load balancerL CloudFront connects to the application load balancer, which sends the incoming packet to an Amazon EC2 instance. 

### AWS Global Accelerator
-  Global accelerator is a service that uses the AWS Global network to improve application availability, performance, and security. It uses intelligent traffic routing and fast failover if something goes wrong in one of our application locations.
-  Global Accelerator is a networking service that helps our applications run faster and more reliably across the globe. We can think of it like creating exress lanes on the internet highway specifically for our application's traffic.
-  Instead of our users' requests taking the regular, sometimes congested internet routes, Global Accelerator directs traffic through the AWS private global network - getting our users to our application faster and more reliably.
-  Use cases:
   1. Global gaming company:
      - A gaming company uses Global Accelerator to reduce lag and provide smoother gameplay for players around the world. Players in Tokyo, new york, and london all experience similar, responsive gameplay because their connections are optimized.
   2. Financial services application: 
      - A banking app uses Global Accelerator to ensure their customers always have fast, reliable access to their accounts. Even during peak times or when network conditions in one area are poor, customers can check balances and make transactions without frustating delays.
-----------------
- Lets review each of the edge services:
  1. Amazon Route 53: Route 53 is a highly available and scalable cloud DNS service.
  2. Amazon CloudFront: CloudFront is a CDN service that delivers our content with low latency and high speeds.
  3. AWS Global Accelerator: Global Accelerator is a service that uses the AWS global network to improve application availability, performance and security. 

## Global Architectures
-  Until now, we worked on creating single VPC in a single region. However, in the real world, companies often need more complex networks to support global customers. This can mean multiple AWS accounts, multiple AWS Regions, multiple VPCs.. even hybrid cloud deployments.
-  Let's start with common setup - a VPC with a VPN connection. This setup allows a company to securely connect their on-premise network to their cloud-based resources on AWS. This essentially creates private, encrypted tunnel to access data and applications in the cloud from their physical office locations, while maintaining data security and privacy over the public internet. Companies often use this for remote employees who need to access sensitive information that is stored in the AWS cloud.
-  While, there are definnitely benefits to using a VPN to connect to VPC, and it works great for many customers, it does also have a few potentials limitations. One is that VPN  connections do share the bandwidth of the local internet, so the connection can be prone to slowdowns if we have heavy payloads of data being sent over the internet to AWS. second might be, our company requirements to meet certain compliance or regulatory standards. Due to these limitations, we might consider using Direct Connect.
-  Direct Connect is also awesome when lots of data needs to flow between corporate data centers and AWS. These huge data transfers can take long time over the public internet so some companies opt for Direct Connect instead. Traffic will be routed from their corporate data center to Direct Connect location. It is then routed to a VPC through a virtual private gateway. All network traffic flows through this dedicated private connection. This helps to speed up data transfers, address application performance and increase a company's data transfer security.
-  When to use VPC and Direct Connect:
   - VPN: We should use VPN when we need a secure, fleible connection for remote access to our resources. This is especially true for small-scale data transfers or when a dedicated connection is not necessary.
   - We should use Direct Connect when we need much higher bandwidth with a dedicated line like with large data transfers between our on-premise network and AWS.
   - A common use case for using VPN alongside AWS Direct Connect is where we use VPN as failover for Direct Connect. 
-  So, with Direct Connect, these are physical hard-wired connections. So, if there is some situation where a line gets cut accidentally or if something were to happen, we can use VPN as backup connection for failover. But there are even cases where we may want to failover to a secondary direct connect line.
-  In addition to fault tolerance, if a customer wants increased bandwidth, they can vombine multiple connections to achieve higher aggregate bandwidth.
-  For companies with customers around the globe or even offices in different regions where they need to deliver content, they could use Amazon Cloudfront and Route 53. CloudFront distributes content from edge locations globally, while Route 53 uses its latency-based routing capabilities to direct users to the closest AWS region (based on their location). This ensures they access the application with the lowest latency, which effectively provides a seamless experience across multiple regions.
    - **Example**: User access the company's website using a custom domain, the request is then sent to Route 53 DNS record. Route 53 uses a routing policy to determine which region is closest to the user and then directs them to appropriate CloudFront edge location. The edge location then fetches the content from the designated origin server in the chosen region.

### Cloud in rwal life: Exploring the examples
#### Direct Connect failover when we need much higher bandwidth with dedicated lines
- Let's learn more about using Direct Connect for failover and to aggregate bandwidth:
  <img width="395" height="179" alt="image" src="https://github.com/user-attachments/assets/8ea950ea-bf50-4cde-b4ac-7f8f50729ee8" />

  1. **Customer Network:** The customer network clients and servers need a secure, high-bandwidth connection for large data transfers and critical application performance.
  2. **Content router or firewall:** The customer has a content router or firewall connecting their network to Direct Connect.
  3. **Multiple Direct Connect Connections:**In addition to fault tolerance, the customer wanted increased bandwidth. They can even combine multiple connections to achieve higher aggregate bandwidth.
  4. **Virtual Private Gateway:** Using a virtual private gateway, the clients can securely access the private resources in the Amazon VPC.
 
#### Delivering content to several different Regions globally
- Below shows an example of how a company with offices around the world can deliver content with low latency for seamless experience across multiple Regions.
- Let's learn about how traffic gets to Regions through CloudFront and Route 53:
  <img width="437" height="230" alt="image" src="https://github.com/user-attachments/assets/5ef535cb-2302-4710-aef0-49b478bee518" />

  1. **Users:** The users access the company's website using a custom domain. The request is first sent to Route 53 DNS record.
  2. **Routing policy:** Route 53 uses a routing policy to determine which Region is closest to the appropriate CloudFront edge location.
  3. **Direct to edge locations:** Route 53 directs the user to the CloudFront edge location in the appropriate Region.
  4. **Content in multiple AZs:** The content is fetched from designated origin server in the choosen region. Also note, the website was built with resources in multiple availability zones for high availability. 

## Summary:
- **Amazon VPC:** Amazon VPC is a service to provision a logically isolated section of the AWS Cloud where we can launch AWS resources in a virtual network that we define
- **Subnet:** A subnet is a section of a VPC that can contain resources and is used to organize our resources. They can be either public or private.
- **Internet gateway:** An internet gateway is a connection between a VPC and the internet. It allows public traffic from the internet to access our VPC.
- **Virtual private gateway:** A virtual private gateway is the component that allows protected internet traffic to enter into VPC. It allows a connection between our VPC and private network only if it is coming from an approved network.
- **AWS Client VPN:**Amazon Client VPN is a networking service we can use to connect our remote workers and on-premises networks to the cloud. It is fully managed, elastic VPN service that automatically scales up or down based on user demand.
- **AWS Site-to-Site VPN:** AWS Site-to-Site VPN creates a secure connection between our data center or branch offices and our AWS Cloud resources.
- **AWS PrivateLink:** AWS PrivateLink is a highly available, scalable technology that we can use to privately connect our VPC to services and resources as though they were in our VPC.
- **AWS Direct Connect:** AWS Direct Connect is a service that provides a dedicated private connection between our data center and a VPC.
- **Network Access Control List (network ACL):** A network ACL allows or denies specific inbound or outbound traffic at the subnet level using stateless packet filtering.
- **Security Groups:** Security groups control the inbound and outbound traffic for a resource at the instance level using stateful packet filtering.
- **Domain Name System (DNS):** DNS translates human readable domain names to machine readable IP adddresses.
- **Amazon Route 53:** Route 53 is a scalable and reliable DNS web service that helps developers and businesses route end users to internet applications, whether they're hosted in AWS or elsewhere. It also supports domain registration, health checks, and advanced traffic routing policies.
- **Amazon CloudFront:** CloudFront is a web service that speeds up distribution of our web content to our users through a worldwide network of data centers called edge locations. It securely delivers content with low latency and high transfer speeds.
- **AWS Global Accelerator:** Global Accelerator is a networking service that helps improve the availability and performance of applications for global users by routing traffic through the AWS global network. It helps improve application availability, performance, and security.
- **Amazon Transit Gateway:** Amazon VPC Transit Gateway is a network transit hub used to interconnect VPCs and on-premise networks.
- **NAT Gateway:** Network Address Translation (NAT) gateway allows instances in a private subne to connect with services outside our VPC. External services can't initiate a connection with those instances.
- **API Gateway:** The Amazon API Gateway is an AWS service for creating, publishing, maintaining, monitoring, and securing APIs at any scale. It handles all the tasks involved in accepting and processing up to hundreds of thousands of concurrent API calls. 
