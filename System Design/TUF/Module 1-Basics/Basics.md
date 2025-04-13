# Basics

## What is system design:

![alt text](image.png)

- Vertical Scaling: Optimize processes and increase throughput using the same resources
- Resilience: Recovering from failures quickly
- Preprocessing using cron jobs: Preparing beforehand during non-peak hours
- Backup server: Keep backups and avoid single point of failure (Master slave architecture for databases or servers)
- Horizontal scaling: Hire more resources

![alt text](image-2.png)

- Microservice architecture: Make separate services based on feature or specialty. Here, we have well defined responsibilities, and we don't have anything outside of the business use case that we handle for each service. 
- Distributed systems (partitioning): When the system is not working, due to some reasons, we should have a backup, like the distributed system. In case of failure, the requests will be routed to the backup system. This is known as fault tolerance. The users are spread all around the world. So, in order to given the quick response, we need local systems/ servers everywhere. This is done by this. 

![alt text](image-4.png)

- Load Balancer: Customer sends the request, it has to be routed to one of the servers/ systems. Depending upon the response time from the servers, the request will be routed to the one with quick response time. This is done by load balancer.

- Decoupling: Here, we can see the separation of responsibilities in order to handle the separate systems more efficiently.

![alt text](image-5.png)

- Logging and Metric calculation: We can observe the events and processes occurring using this.
- Extensible:  We should be able to extend the system features and functionality.


### High Level Design:
- It involves deploying on servers, figuring out how 2 systems will interact with each other.
1. Overload: Horizontal scaling
2. Complexity: Separation of concerns or responsibilities
3. Mishaps: Fault tolerance


### Low Level Design:
- It involves the way we implement the system in code like making classes, objects, functions etc.

## Horizontal vs. Vertical scaling:

![alt text](image-6.png)

- We have written a code, accepting the request and returning the response. We want to expose our code to others using protocol which will run on the internet.
- We will be exposing our code using API(Application Programming Interface).
- Here, the API will receive the request and will return the corresponding response.
- Here, we might need the database connected to it (Its within the desktop itself), the endpoints through which it is being accessed needs to be configured. 

![alt text](image-7.png)

- We also needs to deal downtime situation, like power loss or any other. In order to deal with this, we can host our service on cloud. There is not much difference between cloud and on-premise setup. Cloud is a set of computers which is provided to us, with pay as you go service like AWS, Azure, and GCP etc.
- Cloud provides computation power. It is the desktop provided to us, which will be able to run our code or algorithm. We can store our code or algorithm in it, using the remote desktop login. It is not just single desktop, but a set of computers which we can use to run our service.
- We migrate our code or algorithms to cloud, because of the configurations, settings, reliability is taken care of to large extent by the cloud solution provider .
- Now that we have our server hosted on the cloud, we can focus more on the business requirements.

![alt text](image-8.png)

- Let's say, the number of users using our service has increased drastically. And our service is not able to handle these many connections.
- The solution to this is to introduce scalability. Below are the mechanisms using which we can increase the scalability: 
  a. One of the solutions for this is to buy a bigger machine (Vertical scaling)
  b. Another solution is to buy more number of machines (Horizontal scaling).
- Scalability: The ability to handle more requests by buying more machines, or by buying bigger machines is known as scalability.


### Horizontal Scaling:

![alt text](image-9.png)

- Here, we need load balancing. 
- Resilient: As there are more than one machines, if one of the machines fails, we can redirect the request to others.
- Network calls(RPC- Remote Procedure Calls): Communication between the servers will be over the network. And network can be slow.
- Data Inconsistency: 


### Vertical scaling:

![alt text](image-10.png)

- Here, we have only one machine. So, we don't need a load balancer.
- Dingle point of failure: As there is only one machine present, there is single point of failure.
- Here, we have inter-process communication. So, it is quiet fast.





