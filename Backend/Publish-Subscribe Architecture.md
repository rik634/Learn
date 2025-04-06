# Publish Subscribe Architecture:

Both REST and Message queue boil down to the basic elements. Basically, REST being HTTP which is request response and message queue being publish subscribe system.

## Where Request Response Breaks
![image](https://github.com/user-attachments/assets/4267f3cc-5443-4187-a9c8-9f96aa616c88)
- Suppose a client makes a GET request to the web server. Now, the client will not be blocked. Here, we send asynchronous requests. So, client can make the request, and can do other stuffs in background.
- When we get the response back, we deal with the content of it.
- Here, always client initiates the request.
- This is the request-response model.
- Suppose, we have a system, where we want to upload a video. We dont just upload the video, there is a lot of stuff happening in backgroudn.
- After this, the video is compressed, and pciked by the format service.
- Format service involves producing different video ties for the appropriate devices.  Suppose for a mobile phone content, we need 480, or 720, or 1080 etc. Here, we want to produce different kind of content based on the viewing platform.
- After the upload is done, we want to notify the subscribers about it. This is done by the notification service.
- We can;t notify people, once the video is uploaded. We have to notify them, once the video is ready to be consumed.
- After getting the response from notification service, it goes to format service, which gives response to compress service, and upload service. And finally client gets teh response, that the video has been uploaded and notified to subscribers abotu it successfully.
- If any one of this steps breaks, whoel things is broken. The reason can be any obstacle, or network error.
- This is one of the cons of the request-response model. If we are chaining multiple services, especially in microservice architecture, that breaks down.

![image](https://github.com/user-attachments/assets/6765d5c1-3ed7-4c37-af76-a26a1eed39c9)

- Now, we want to add another service called copyright service to check the content for copyright infringement.
- Here, the compress service, needs to send the data to both format service and the copyright service.
## Request Response Pros and Cons
### Pros
- Elegant and simple
- Stateless (HTTP)
- Scalable: Can be scaled horizontally. It is scalable at the receiver end where we can duplicate the receiver if it is the same content or functionality. And we can put it behind the load balancer. Its not scalable in other terms. scalabiltiy is overloaded here. 

### Cons:
- Bad for multiple receivers: In case of multiple recievers, the architecture might fall apart. 
- High coupling: Due to this, services start talking with each other, but, we want services to be as oblivious as possible about the whole system.
- Client/Server have to be running
- Chaining, Circuit breaking
## Publish Subscribe Architecture

![image](https://github.com/user-attachments/assets/df4c1a17-1aff-4a8b-af75-e296c47c4a81)

- 
## Pub/Sub Pros and Cons
