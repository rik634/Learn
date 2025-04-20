# Load Balancing
- The concept of taking N servers nd trying to balance the load evenly on all of them is known as load balancing.
- The term consistent hashing helps to evenly distribute the incoming load among the N servers we have.

### Hashing Requests:
- We get a request ID in each request. When we send a request from the client side, it randomly generates a number between 0 to M-1. This request ID is sent to the server. 
- When we hash that request ID, we get a number. This number is used to map the request to a particular server. 

![alt text](image.png)


## What is Load Balancing:


## What is consistent hashing:


## What is sharding:
