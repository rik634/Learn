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
- You can use Lambda to run code! With Lambda, you don’t need to think about servers or clusters. You create a Lambda function, put your code in there, configure a trigger, and your function runs in response to that trigger.
- Triggers can be more complex, though. Like processing data in real-time from a stream or resizing an image into different resolutions. And the claw-some part is that you don’t have to manage the environment. AWS handles it all for you!
- 
