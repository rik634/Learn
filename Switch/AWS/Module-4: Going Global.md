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
   - 
