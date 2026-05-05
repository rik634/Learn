# Exploring Compute Services

## Introduction to Serverless Computing
- With serverless computing, we run applications without managing the underlying infrastructure.
- There are different types of compute services in AWS: unmanaged, managed, and serverless 
- Some of the serverless compute services in AWS are:
  1. AWS Lambda: A serverless solution that automatically handles scaling, availability, and maintenance.
  2. Amazon ECS (Elastic Container Service)
  3. Amazon EKS (Elastic Kubernetes Service)
  4. AWS Elastic Beanstalk: It make container management and application deployment more manageable.  
-  Unmanaged service:
   - We know, how Amazon EC2, an unmanaged service, gives us full control over the virtual machines. EC2 instances are virtual machines  that we can provision on AWS.
   - EC2 is great for all sorts of use cases from running basic web servers to high performance computing workloads, and everything in between.
   - EC2 offers a high degree of control when it comes to our instances, but it also requires that we manage that fleet of instance over time.
   - EC2 is an unmanaged service, means we have control over tasks like patching, scaling, and managing the operating system, while AWS manages the underlying infrastructure.
    
-  Managed service:
   - Managed services shift more operational responsibilities to AWS- so we can focus on building our application and less on managing infrastructure.
   - Some examples of managed services are; ELB, SNS, and SQS.
   - For managed services we configure the service to meet our requirements, and then AWS makes sure it runs smoothly over time, with no server management required on our part.
   - This idea of not managing any underlying infrastructure led to the rise of serverless computing. 

- Serverless:
  - Serverless means that we can't actually see or access the underlying infrastructure or instances that are hosting our application.
  - Instead, all the management of the underlying environment from provisioning, scaling, high availability, and maintenance perspective are handled for us.

### Unmanaged and managed and serverless services:
- With unmanaged compute services like Amazon EC2, AWS takes care of the underlying physical infrastructure, but you're responsible for setting up, securing, and maintaining the operating system, network configurations, and applications on your instances.
- Managed services, on the other hand, reduce the amount of infrastructure you need to manage. While AWS handles much of the operational overhead, you might still need to perform some provisioning or configuration depending on the service.

<img width="809" height="418" alt="image" src="https://github.com/user-attachments/assets/da034eec-12bb-4e87-bc9a-51746366e73e" />
  
- Fully-managed services—like serverless ones—take abstraction even further, eliminating the need to provision or manage any servers at all. The underlying infrastructure is fully managed by AWS, so you can focus entirely on writing and deploying code.
  - Example: Lambda is a serverless compute service where AWS handles the infrastructure, scaling, and availability, while you remain responsible for securing and managing your application code.

## Amazon Lambda
- AWS Lambda, a serverless compute service that runs code without managing servers. It’s also known as a Function as a Service.
- Lambda is a serverless compute service that runs code in response to events without the need to provision or manage servers.
- It automatically manages the underlying infrastructure, scaling resources based on the volumne of requests. 
- You can use Lambda to run code! With Lambda, you don’t need to think about servers or clusters. You create a Lambda function, put your code in there, configure a trigger, and your function runs in response to that trigger.
- Triggers can be more complex, though. Like processing data in real-time from a stream or resizing an image into different resolutions. And the claw-some part is that you don’t have to manage the environment. AWS handles it all for you!
- This managed environment is automaticcaly scalable and highly available. This means whether we get a single trigger or thousands, Lambda will scale up or scale down to meet the demand. Here, AWS takes care of all the patching, updates, and security. We just have to focus on code, and the maximum duration of Lambda function is 15 minutes. If we can't split our code into 15 minutes segments, lambda might not be the best fit.
- Lambda is great for quick, event-droven processes. Example: Handling website requests, processing batches of data, and generating expense reports.
- Lambda supports any programming language by default through the use of runtimes.Example: Java, python, and Node.js
- We can aslo build our own custom runtime. A runtime provides a language-specific environment that relays invocation events, context information, and responses between lambda and the function.
- Lambda integrates with other AWS services quite easily.
-  Here, we are charges only for the compute time consumed, down to the millisecond. Lambda handles execution, scaling, and resource allocation. We can optimize performance by configuring the appropriate memory size for your function.

### How Lambda works:
- The steps are as follows:
  1. Upload code to Lambda: First, upload the code to Lambda, which uploads as a Lambda function
  2. Set code to trigger from an event source: Next, configure our code to be triggered by events, like AWS services, mobile apps, or HTTP requests.
  3. Run code when triggered: Our code runs only when an event occurs, like a file upload or user action. Lambda automatically handled all the server management, scaling and infrastrcuture. The Lambda runtime executed our function code using the event data passed to it. This way, our code runs reliably, securely, and efficiently - eihout us managing the servers or environment it runs it.
  4. Pay only for the compute time used: We are charged only for the compute time consumed, down to the millisecond. The price depends on the amoutn of memoyr that we allocate to our function. 

### Lambda use cases:
- Lambda is ideal for building responsive, event-driven applications across a wide range of industries.
- Below examples show how Lambda helps companies scale efficiently, reduce operational overhead, and only pay for what they use:
  1. Real-time image processing for social media application:
     - A social media company uses Lambda to process images uploaded by users. When a photo is uploaded, Lambda is triggered to resize the image, apply filters, and save it in an optimized format to storage. This event-driven, serverless approach makes sure that the application can handle high volumnes of uploads without needing to manage infrastructure.
     - Why Lambda: It automatically scales based on uploads and charges only for the time spent processing each image. 
  2. Personalized content delivery for news aggregator:
     - A news aggregator uses Lambda to fetch and process new articles from multiple sources, then it tailors recommendations based on user preferences. When a user opens the application or performs a search, Lambda functions are triggered to retrieve data, run personalizaton logic, and return relevant content.
     - Why Lambda: It automatically scales with user traffic and reduces costs by running code only when user interact. 
  3. Real-time event handling for an online game:
     - A gaming company uses Lambda to handle in-game events like player actions, game state changes, and real-time leader board updates. Each event (like scoring a point or unlocking an achievement) triggers a Lambda function that updates player data and game status.
     - Why Lambda: It handles thousands of events, in real-time with no need to manage servers. Costs scale with usage, which is ideal for peak gaming times. 

### Lambda demonstration:
- Here, we will connect Amazon SQS with Lambda to build an event-driven workflow.
<img width="1238" height="525" alt="image" src="https://github.com/user-attachments/assets/8cd2a4c7-84a6-458c-a33c-7f3d11ecbdfd" />

- Here, in above architecture, SQS queue automatically triggers a Lambda function whenever a new message is added to it. Even for this simple-looking workflow, we must ensure the Lambda functions has the right permissions to access the SQS queue.
-   we can create a SQS queue and Lambda function (from bkueprint), and we need to set up an executinon role to alloe lambda function to read messgae from SQS queue. We can use Amazon SQS poller policy template, which will allow lambda function to pull messages from queue.
-   Now, we will select thr trigger for lambda function, which is SQS, and select the created SQS queue.
-   In Lambda consile, we can see function metrics by going to the monitor tab and tgen clicking View CloudWatch logs. We then click on the log groups. We click on the function name. We open up the log streams. Adn we can see, for the log stream, we have our messages from SQS were processed successfully by lambda function.

## Containers and Orchestration on AWS
- Containers - a powerful way to package and run applications consistently across different environments, with the flexibility to scale and deploy efficiently on AWS.
- Containers solve the portability problem (i.e., application deployment working on one system but failing on another system/machine) by providing a onsistent environment that can be replicated anywhere. This is what containers do.
- Container packages everything our application needs to run - code, runtime, dependencies, configuration - into a single, portable unit.
- This creates a consistent environment, isolating our application forom the underlying system and making it convenient to deploy and scale our application anywhere.
- Containers also provide benefits like faster start times and improved resource efficiency.
- Now, let's talk about hosting the containers:

  - We can manage containers on our own, where we place containers on top of a cluster  of EC2 instances, but it is a lot of work.  We'd have to deal with monitoring the health of the containers, starting and stopping them when needed, updating them, and managing the networking for them, and more.  We can manage it, but it would be complicated. This is where container orchestration services comes in.
  - Container orchestration services manage the lifecycle of containers, including starting, stopping, and running them across a cluster. These orchestration services automatically scale containers out when traffix increase s- and scale back in when things calm down.
  - This way, our application can hanlde spikes in demand without breaking a sweat. It also handle recovery from failure, monitoring, and upates, saving us tons of time and effort.
  - On AWS, we have 2 main container orchestration options: Amazon ECS and Amazon EKS. 

### Amazon ECS:
- Amazon ECS stands for Amazon Elastic Container Service. It's ideal if we want something streamlined and integrated, but we can still define our application's container images and resources, such as EC2 instance types and load balancers.
- ECS automatically manages the containers and their infrastructure based on the parameters we set. 

### Amazon EKS:
- Amazon EKS stands for Amazon Elastic Kubernetes Service. Kubernetes is an open source platfrom that automates containerized application deployment, scaling, and management.
- EKS makes it convenient to run kubernetes clusters on AWS.
- It offers a lot of control and flexibility, especially for large-scale or hybrid deployments.

### Amazon ECR:
- Now, orchestration services need somewhere to get their containers from. That's where Amazon Elastic Container Registry, or Amazon ECR, comes in.
- ECR is fully managed (full managed also means serverless) container registry that stores our container images.
- We build our containers that have our application and all of its dependencies bundled together.
- From there, a container orchestration tool can pull the container image and deploy it.

- Now, let's understand, where our container will actually run. AWS offers 2 options for this:
  1. Amazon EC2:
     - With EC2, we manage the virtual machines that run our containers. With this option, we have full control, but we need to manage the underlying infrastructure.  
  2. Amazon Fargate:
     - Fargate is serverless and offers efficiency and convenience.
     - With Fargate, AWS manages the servers, and we only need to worry about our containers.
     - No need to manage a fleet for these containers to run on.  

- So, first, we will upload our container images to ECR, which is where our images get stored securely and are ready to be used. After that, we will pick an orchestration service based on what we need, either ECS or EKS. Then,  we will choose our compute options, either EC2 or Fargate.

### Solving deployment challenges
- Containers provide reliable way to package our application's code and dependencies into a single, portable unit, making them ideal for workflows that require high security, reliability, and scalability. 

#### Containers and VMs
- A container packages our application with everything it needs to run, so it works the same on any computer.
- This helps to move, update and manage.
- 
