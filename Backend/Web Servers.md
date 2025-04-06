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

### Examples:
- OTS Web servers (httpd, IIS, lighttpd, tomcat, http-server)
- Write your own server (nodejs, python tornado)

- If we use IOS, we can just write an ASP application, ASP stands for Asp.net Active Server Pages.

##
### Apache-2 and Httpd:
The terms "httpd" and "Apache 2" are closely related but not exactly the same.
- Apache HTTP Server (or Apache 2): This refers to the entire web server software package developed by the Apache Software Foundation. Apache 2 is simply the second major version of this software and is widely used as a web server to serve websites over HTTP/HTTPS.
- httpd: Short for "HTTP Daemon," this often refers specifically to the executable or process of the Apache web server that handles HTTP requests. It's essentially the "engine" running behind the scenes. On some systems, the Apache executable is called , which is why people sometimes use the terms interchangeably. 
##
##
### Apache web server:
he Apache web server is essentially a "ready-made" web server software. It's a pre-built, widely used open-source solution that allows you to host and serve web content (like websites, APIs, or applications) without having to build a server from scratch.

Here’s why it’s considered ready-made:
- Preconfigured Capabilities: It comes with essential web server features, such as handling HTTP/HTTPS requests, serving static files, support for dynamic content using modules like PHP, and more
- Customizability: Even though it's ready-made, it’s highly configurable. You can modify its settings (like virtual hosts, modules, and more) to suit your specific needs
- Ease of Use: It provides out-of-the-box functionality to get a basic web server up and running quickly, while still offering advanced features for complex projects

### Installation:
This is the installation for Linux.

![image](https://github.com/user-attachments/assets/7b2bbf2a-fd90-460a-9903-6504952441e6)

- Now, it is installed.

![image](https://github.com/user-attachments/assets/3d13af21-0e5d-4a2d-bb4d-d9ebb120412e)

- Go to the path mentioned in below image:

![image](https://github.com/user-attachments/assets/caf30c64-bd85-4fc1-97f2-9e7ef3780269)

- Here, we have a file for test purpose.
- By default, Apache-2 works on port 80. Let's try to run that page on browser.

![image](https://github.com/user-attachments/assets/d7e584d3-23fa-4c9b-8fab-1c8af1cf024b)

- It is working.
- Here, the owner of the html file or the folder is PIP, and not the user. Let's change the owner.
- First go one folder back:

![image](https://github.com/user-attachments/assets/705f80e6-7b52-4c4f-affa-654e4dfd1510)

- Change the owner of tg=his folder:

![image](https://github.com/user-attachments/assets/7f8b88fa-5a82-4067-8eae-cdbcd1776c86)

- Remove the previous html file and create a new file:

![image](https://github.com/user-attachments/assets/e3871b42-7f89-431d-8446-283906ac78f7)

![image](https://github.com/user-attachments/assets/4e1f2041-7641-4fe9-a41a-09d51d6bea67)

- We can observe the output on the browser:

![image](https://github.com/user-attachments/assets/edbf3f37-0ff2-48bd-8b57-bc942fb295ec)

- Let's create a "main.js" file.

![image](https://github.com/user-attachments/assets/c0f1becf-eec7-49bf-a374-57075c532531)

![image](https://github.com/user-attachments/assets/6d47f1bf-3ddf-4d7f-920b-2fbadbb1978c)

- Lets adit index.html file to use this "main.js" file.

![image](https://github.com/user-attachments/assets/6fcad97e-a109-45a9-8516-dce64f20581a)

- We can observe the output on the browser.

![image](https://github.com/user-attachments/assets/6775bd56-1547-46e4-a927-f2975f4de971)

- First of all, it will get the index.html file:

![image](https://github.com/user-attachments/assets/a704ff72-5b20-4ab4-b146-2e17afa8a0c3)

- When parsed by the browser, it discovered that we need one more file needs to be pulled. so it goes to the TCP connection:

![image](https://github.com/user-attachments/assets/5949abf9-b3db-43bc-91b2-9b7c7c86d9a4)

- Getting the data is extremely slow, because every connection request, will start the TCP cache and close it at the same time.
- Here, we have something called "connection: keep-alive:"

![image](https://github.com/user-attachments/assets/62939a8e-3ded-4b2b-b5ce-84a42771bc59) 

- It means to keep the connection alive.
- When we make a request to any content for first time, we get back an e-tag, and client use that e-tag with every single request further.
- Using that cache, it gets to know if any changes are there in the content. This is kind of cache.
- if the content did not change, the status code will be 304(Not Modified).

![image](https://github.com/user-attachments/assets/161c2b8a-0130-4c3d-9631-34eba585adff)

- Now, let's change the "main.js" file.

![image](https://github.com/user-attachments/assets/375886e6-87dc-4e42-81cb-c2eb292d2fb6)

- During sending the request, we sent the same e-tag as earlier:

![image](https://github.com/user-attachments/assets/ec1ac37f-6d2e-461f-8d5d-4d585b6017e9)

- But, in response, we got different e-tag.

![image](https://github.com/user-attachments/assets/1eb6a748-6a17-4ad2-9c6f-b6f53b8c3395)

- Status code is also changed:

![image](https://github.com/user-attachments/assets/1750cd61-2963-4618-a5a2-ae466361f871)

##

## Web server using NodeJS:
- HTML page:

![image](https://github.com/user-attachments/assets/4ef0c7e2-4c0f-4aa0-9cfa-ddc0755e080b)

- We will use 8080 port number, as 80 nis reserved for highly priviledged apps.
- index.js file:

![image](https://github.com/user-attachments/assets/6bc4339d-0bb1-479e-952b-1db6523a5ca7)

- when u select debug option, we get below message on console:

![image](https://github.com/user-attachments/assets/df3128f6-902b-4947-860f-10474d7728f6)

- If we check localhots on browser, we get:

![image](https://github.com/user-attachments/assets/45163a18-2998-4aae-b399-31e5dd4b8e81)

- This is because, we did not implement anything, other than, getting the express package. and listening on port.

![image](https://github.com/user-attachments/assets/725acc6b-dee7-4d86-afd9-9c129ebb4339)

- This says that there is nothing on the server and server is dead.
- so, we need to implement the get method on routes, to accept the request.
- Here, we are srving the index.html page: Here, we dont have anything in the request. We just want to send this index.html file, if someone visits on the path mentioned.

![image](https://github.com/user-attachments/assets/157e9259-1bcc-4914-b505-806c851ca8cd)

- Here, we can observe the content being rendered on page:

![image](https://github.com/user-attachments/assets/d8a8e4d4-513f-4ccf-87a6-922aeca561f3)

- Let's add one more function:

![image](https://github.com/user-attachments/assets/8e42ae50-695a-4b01-a41b-e90b3c845ac1)

- We get below output:

![image](https://github.com/user-attachments/assets/2335604a-d0c6-4aab-b757-3fcc2c2d24b6)

![image](https://github.com/user-attachments/assets/2b8c8ef9-e2fd-473f-bcbe-a142fe851546)

- We can change the status code internally using code as well:

![image](https://github.com/user-attachments/assets/11bbcdb7-97ce-4e7e-8adb-223964eafa54)

![image](https://github.com/user-attachments/assets/9fdd1513-b5d4-4fd3-8187-4ac10661f1ef)
- 
