# Going Global

## Introduction to Going Global
- When we are expanding globally, there are a number of factors to consider when selecting AWS regions.
- Edge locations offer fast, localized delivery of the most frequently accessed content. They cache things like images, videos, ad other assets and resources, allowing users to gte the content they need quickly, withuot waiting for it to be retrieved from a central location.
- AWS has infrastructure in place to help businessess scale responsibly and consistently.

### Going global with AWS infrastructure:

#### How to choose a Region or set of regions:
- We have several factors to consider when selecting a region or set of regions for our real-life resources.

#### AWS edge location
- AWS has smaller footprint facilities called edge locations.
- Edge locations cache items like image, videos, and other resources, so that users can access the content they need with lower latency.

#### Infrastructure as code and CloudFormation
-  AWS has services, such as CloudFormation, that we can use to help automate the deployment of our cloud resources.
-  These services use infrastructure as Code, or IaC, helping us to achieve a consistent, reliable set tup each time our business grows.

## Choosing AWS Regions
- In case of AWS Global infrastructure, we have lots of options when it comes to which AWS Region, or Regions, we deploy our resources in.
- Each Regions is isolated from every other Region, in the sense that no data goes in or out of our environment in that Region wihtout us explicitly granting permisssion for that data to be moved.
- Depending on the type of Business we are dealing with and where we operate, we might have to adhere to specific compliance regulations that require our data to remain in one grographical area.
- Example: If we are working with financial information in Frankfurt, local data governance laws state that this financial data cannot leave germany.
- The data that is stored in an AWS Region is subject to the local laws and statues of the country where Region lives. Which is actually our first of 4 considerations when choosing a region: compliance
- The second factor is proximity: How close we are to our customer base is a major factor. If most of our customers live in Singapore, consider running out of Singapore Region. We can certainly run out of Virginia, but the time it takes for the information to be sent , or latency, between the US and Singapore is always going to be a factor.
- The third factor is availability: Sometimes the closest region might not have all the AWS features we want. Every year, AWS releases lots of new featured and products speciafically to answer customer requests and needs. These features are rolled out to regions over time, so that is also a consideration.
- The fourth factor is Pricing: Even when the services and features are equal from one regions to the next, some locations are more cost effective to operate in than others. Thingsa like local tas structure and energy costs factor into the equation. AWS has a very transparent, granualr pricing. Each region has different numbers for pricing.
- We have to keep these factors in mind when choosing a region: compliance, proximity, feature availability, and pricing.

### Key considerations when choosing regions:
1. Compliance:
   - Compliance is an important consideration when selecting regions for deploying business resources.
   - Different geographical locations have varying regulatory requirements and data protection laws that organizations must follows.
   - For Example, the General Data Protection Regulation (GDPR0 is designed to protect the personal data and privacy of individuals withing the European Union (EU). An online retail company operating in EU would be required to meet GDPR compliance to protect customer data. GDPR compliance includes obtaining proper consent for data collection and providing mechanisms for data access and deletion. 
2. Proximity:
   - When selecting a Region, we also want to consider how to achieve low latency for our users.
   - Regions closer to our user base minimize data travel time, which reduces latency and enhances application responsiveness.
   - Choosing a Region ot set of Regions farther away from customers coudl introduce delays, which might impact user satisfaction and overall system efficiency. 
3. Feature availability:
   - We also want to consider which specific features and services are available in each Region.
   - AWS is constantly expanding features and services to multiple locations, but not all regions contain all AWS offerings.
   - For exmaple: AWS GovCloud regions are specifically designed to meet compliance and security requirements of US government agencies and their contractors.These regions have stringent physical, operational, and personal security controls in place. These controls are only avaialable in speciffic regions to meet certain governmental regulatory requireemnts. 
     
4. Pricing:
   - When selecting a region, pricing is also a factor that can influence our decision.
   - Some regions have lower operational costss than others. These operational costs can impact the overall expenses for hosting applications and services.
   - Tax alws and regulations can also play a role in cost. Some regions might offer tax incentives or have lower tax rates, which can affect customer pricing.
   - Additionally, data sovereignty laws in certain regions might require data to be stored locally, affecting both compliance and cost. 

## Diving Deeper into AWS Global Infrastructure
- High availability and fault tolerance means that our resources remain accessible and operationsl, even if an outage occurs in one of the locations hosting our resources.
- When it comes to infrastructure, we want to plan for long-term stability and less or virtully no down time for our users. SO, if our infrastructure has an interruption, we can swicth to redundant or backup infrastructure seamlessly. This is called building redundant architecture.
- One method for redundancy is an architecture that uses multiple Availability Zones, or AZs. In a multi-AZ architecture, if an AZ has an  interruption, no worries. Our application will automatically swicth over to backup AZ we have configured. Even better, if we set it up correctly, our customers won't even notice a difference. Additionally, multi-AZ architectures can assist with quicker disaster recovery, improved business continuity, lower latency, and compliance.
- We can deploy our application in multiple AWS Regions. So, if a whole Region experiences an interruptions, we can failover to another one.

### Amazon CloudFront
- CloudFront is a content delivery network, and it's designed to serve content as close to users as possible
- This content can be images, data, videos, applications, APIs.
- CloudFront uses edge locations, which are part of our worldwide Amazon Global Edge Network. These edge locations are actually separate from Regions and are specifically designed to accelerate content delivery.
- Edge locations host other AWS services, like AWS Global Accelerator and Amazon Route 53.

### Amazon Route 53:
- route 53 is a Domain Name System , or DNS, that routes end users to internet applications.
- Essentially, it converts human-readable URLs to machine-readable IP addresses.

### AWS Outposts:
- let's say that, we need more speed than even a Region paired with cloudFront can achieve. This is where Outposts comes in handy.
- Outposts essentially makes it possible for us to run AWS services on-premises.

### Designing highly avaialble architectures;
#### Deploying multi-Region and multi-AZ resources
- As we know, deplpying our cloud resources to multiple Regions can achieve high availability.
- In addition to deploying to mltiple Regions, we also want to deploy resources to multiple Availability Zones.
- By building redundant architectures or replicating our resources across multiple levels of AWS infrastructure, we can improve application reliability so that our users have access to our content when they need it.
- In additiona to high availability, the AWS Global infrastructure also helps us to achieve agility and elasticity for our business.
- Below are the advantes:
  1. High availability:
     - High availability refers to the capability of a system to operate continuously without failing. In the context of AWS infrastructure, it means that our application can handle the falure of individual components wihout significant downtime. 
  2. Agility:
     - Agility refers to the ability to quickly adapt to changing requirements or market conditions. With AWS infrastructure in place, we can modify and deploy services rapidly. 
  3. Elasticity:
     - Elasticity refers to the ability of a system to scale resources up or down automatically in response to changes in demand. AWS infrastructure is set up for us to scale resources up and down on demand. 

#### Edge Locations
- In addition to AWS Regions that contains Availability Zones, AWS has a global edge network that provides quicker content access to users outside of standard Regions.
- These edge locations are strategically placed in areas like Atlanta, Gerogia, USA or Shanghai, China to provide low-latency access to AWS services and content delivery.
- Edge locations offer multiple services to run closer to end users, including AWS networking services like Amazon CloudFront. CloudFront is a content delivery network (CDN) and caching system.

### Key elements of AWS Global infrastructure
#### AWS Regions
- Regions are geographical areas around the world around the world that are made up of multiple data centers.
- These data cemters provide scalable and redundant infrastructure for hosting cloud services.
- Each Region consists of multiple, isolated locations known as Availability Zones.
- Each Region has 3 or more Availability Zones.

#### Availability Zones
- Availability Zones are distinct locations within a Region, each designed as an independent zone with its own power, networking, and connectivity.
- Availability Zones maintain high availability and fault tolerance for applications.
- Each Availability Zones consists of one or more data centers.

#### Edge locations
- Edge locations are strategically placed sites around the world that cache contentto deliver data, video, and applications with lower latency and higher transfer speeds.
- Edge locations are considered a vital part of the AWS content delivery network (CDN) and use services like CloudFront to efficiently distribute data to end users.
- Edge locations are in areas outside of Regions.

## Infrastructure and Automation
- 
