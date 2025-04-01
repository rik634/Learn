# Web Servers
- Web server is a software that serves web content through the HTTP protocol.
- It is foundation of internet. And any website must be hosted by a web server.
- Web APIs like REST API can also be hosted on some sort of variation of the web server.
Examples: Tomcat, Apache (Already built). 
- We can also build our own web servers using the frameworks like: NodeJS, ExpressJS, Python

## What is a Web Server?
- Software that serves web content like HTML pages, PDFs, images, videos, JSON document. 
- Uses the HTTP protocol (HHTP.1, http-2, http-3).
- Static and Dynamic content.
  - In case of static conten, we can't change anything as a user, we can just consume. It is just a bunch of static content, HTML pages.
- Used to host web pages, blogs and build APIs.
- We can host a web server, put the static files on, and cab serve thos easily.
- We can paste the content in the folder of the web server, and web server will immediately generate URLs.
- Static content can be encached.
- Dynamic content is like we query to the database, and it will retrieve certain results. So, based on the logged in user, we can get different content, pages etc.

## How Web Servers work?
![image](https://github.com/user-attachments/assets/f4a0d6ad-20d8-4706-ab13-65e940c7a3f3)

- Let's say we have a client and a web server.
- The client will have the IP address. And the server will have a domain name, host name, and it will be running on a port.
- Usually, this port if 84, for the web servers. It is the default port. If we dont see any port in the URL, by default, it is port 84.
- We can also run the web server on any port we want.
- Normall, the standard followed is: 84 HTTP, 443 HTTPS
- when we have a web server running and listening on this port, there is a TCP socket or connection ready.
- If we make a request to get index.html page, server will process the request, may be will query to the datbase, can just pull it from cache or from directory as well.  And it will server us back with the result, the response HTML pages as requested.
- Along with the HTML page, it also sends a bunch of stuff such as headers, status code etc.
  - 200: OK
  - 404: Not Found

## Examples
### Blocking Single-Threaded Web Server

### Example-1:
![image](https://github.com/user-attachments/assets/fbc22895-32ea-4ab7-97e7-2f8e0c362608)

- HTTP protocol runs on transport layer called TCP (Transmission Control Protocol).
- TCP establishes 2-way communication between server and the clinet
- HTTP protocol is just request to response.
- When a request is made from the client side, first of all, it will establish the connection. It needs to establish the TLS (for security purpose).
-  After this, server creates a memory. Every connection reserves memory on the server. This is what leads to various attacks like DOS, which happen because of the reservation of memory. There is also a chance of server crash.
-  The memory reserved on server is also known as TCP socket. We can have as many sockets as we want.
-  Here, we have a thread/process that is allowed to execute stuff.
-  There can be many processes in the memory, which can be idle. But, we can execute one process at a time.
-  Suppose we have a thread, and it is busy doing any one of the processes. Now, it can not take any other process, until the current ongoing process is completed.
-  After completing the request, it can take another process. Now, we dont need to create a brand new TCP connection, we can use the previous one itself.
-  We know that when we are creating a TCP connection, we are reserving a memory. If we are not using that connection, we are just wasting the server memory.

### Example-2:
![image](https://github.com/user-attachments/assets/70e9f351-e10d-43fb-9e0d-4b2f03708007)

- Suppose, we have 2 clients. From one client we sent a POST request to the web server to create a new to-do list.
- Server takes the request and starts processing.
- Now, second client wants to connect with the web server. But, the web server is busy processing the request from first client.
- Here, comes the web server implementation. This implementation is same for apache as well as tomcat. When we are creating our own server using NodeJS or ExpressJs, we can write our own implementation for such case.
- Here, the web server will craete a TCP socket/  reserve a memory for the second client, but, can not server it until the current request processing is not completed.
![image](https://github.com/user-attachments/assets/ae8787d8-018b-40ff-83d8-4d972996476d)

- After the processing for first client is done, it will serve the second client.
- Apache uses pooling and reuses threads or processes to balance efficiency and scalability
##
### Apache HTTP server:
Apache HTTP Server does not spin up a new thread for every single request. It depends on the processing model being used:
- Prefork MPM (Multi-Processing Module): Apache creates multiple child processes, and each process handles requests using a single thread. For every incoming request, an existing process from the pool is assigned to handle it. A new process isn't created for each request.
- Worker MPM: Apache creates processes, and each process can handle multiple requests simultaneously using multiple threads. A new thread, not a process, is spawned if needed. This model is more efficient for serving concurrent requests.
- Event MPM: Similar to the Worker MPM, but optimized for handling keep-alive connections. Threads are reused more effectively, reducing overhead.

These models are designed to avoid creating excessive threads or processes, as doing so could negatively impact performance. Instead, Apache uses pooling and reuses threads or processes to balance efficiency and scalability.

In Apache HTTP Server, you typically set limits on both maximum threads and maximum connections, depending on the MPM (Multi-Processing Module) you are using.
##

-      

