# TCP Vs UDP
- TCP and UDP are communication protocols that allows us to send and receive the data in a network.
- TCP and UDP are layer-4 transport portocols.
- IP address; Each machine/VM, if we go all the way down to each container, can have their own
  IP address within a network. and that IP address is the address of that machine. At any time, there are 1000's of applications running into a mahcine, so, we nned to further discriminate it.
  It can done using ports.
- so, to identify the application, we need IP address and port number information.

## TCP pros and cons
- TCP stands for Transmission Control Protocol.
- This protocol was designed to allow us to send information from one server to another server by specifying the IP address and the port number.
- HTTP underneath is actually TCP.
- Databases used TCP all the time. Sometimes UDP is also used by databases.

### Pros
- Acknowledgement: A TCP acknowledgment (ACK) contains the sequence number of the next byte the receiver expects, effectively acknowledging all bytes received up to that point
- Guaranteed delivery:
  - If the sender does not receive the ACK, it will resend the message, which guarantees the delivery.
  - It will continue to resend until it gets the ACK.
- Connection based:
  - Connection based mean that for the client and the server to communicate with each other, they need to first establish a unique connection between them
  - And after the connection is established, client and server can communicate with each other as long as they want it.
  - The process and server hold the informaiton about this connection.
  - Session layer makes sure that all the information/message is delivered in timely manner.
- Congestion control:
   - When we are sending the data, sometimes the packets gets delayed. This can be due to overload of senders, and server might get overwhelmed.
   - This congestion control feature is used to control the traffic, and avoid the delays in the packets sending procedure.
   - When there is a lot of traffic, this congestion control feature stops the data from being sent, keep it on hold. and it will send the data only when the network can handle it essentially.
- Ordered packets:
  - Using the congestion control, some packets will reach the server before other packets.
  - Suppose we want to send a long message, it will be chooped up and sent in form of many data packets. some data packets might reach earlier than others.
  - But, it can't be guranteed that packets will be recieved in order. TCP wil order these packets. In order to order the packets, it will be labeled.

### Cons
- Larger packets:
- More bandwidth:
  - On 2G or 3G network, it is tough using the TCP. This is because packets are larger now.
- Slower than UDP:
  - All these features like Ordered packets, congestion control, make ti slow.
- Stateful:
  - If we can destroy our server, and restart it back, while the client is connected and have the client just resume their work normally without any owkrarounds/interruptions.
    Then system or server is stateless. The client is able to work without any interruptions, because here, we dont store any information.
  - But, TCP is stateful.  This is because, the server carries the information about the connection. if we destroy the server, all the connections stored will get closed. And if the client corresponding to that connection tries to send the information, it will fail, and will show the message saying server is out of reach.
  -  In case of TCP, client won't be able to resume their work, if we restart the server.
- Server Memory (DOS):
  - Because of statefulness, and connection based feature, server has to stack up on the TCP connections. They have to allocate the memory for each conneciton.
  - Also, server has to keep listening the requests, and keep checking on the connections.
  - Here, the number of connections we can make is limited.
  - DOS (Denial Of Service) attcks happens because, someone can try to establish the connection to the server suing the TCP connections. Its uses the pros features against the server.
## TCP Code Example (Nodejs and Telnet)
![image](https://github.com/user-attachments/assets/7b4cb04e-5984-4963-9514-91e677b864a1)
- This is NodeJS code.
- Here, we need a package called "net".
- net is used to create a server. This server will give us a socket. This is our connection essentially.
- Once we create a server, we start listening to that. Once a client connects to us, we write back a "Hello" message back to the client. This is done only once when the client connects to the server.
- when clients sends the data, we call the second function above. This function will receive the data, and prints it on console.
- as mentioned in code, we are listening on the port 8080.
- here, we have a TCP server created.
- Click on debug option, to run the server. The sever will start running, and waits for connection.
- we will use telnet to establish connection, on terminal. 
- we can install it on windows.
  ![image](https://github.com/user-attachments/assets/b26f4ede-1584-40af-95b3-58c2c4b94f81)
- Here, in above image, we can say that connection is established.
  ![image](https://github.com/user-attachments/assets/d45714a7-c790-4868-b592-be4bbc89dd48)
- lets send a work "hi".
- It will trigger the second function.
- we can observe the output on the console:
  ![image](https://github.com/user-attachments/assets/d0138917-c4c1-4bb4-9814-8ad999031845)
- Now, we will close the connection, and try to send the data to server:
![image](https://github.com/user-attachments/assets/9eaeaf06-f624-4698-9201-695d1ec1e0d2)
- here, we can see that, the data is no more stored. This is what we call as statefulness.
## UDP pros and cons
- UDP stands for User Dtatgram Protocol.
- The information which UDP packets adds on top of the messages is CRC(checksum).

### Pros:
- Smaller packets: This is because, UDP wont have headers to maintain the data for the features like congestion control, ACK etc and more.
- Less bandwidth: small packets lead to lower bandwidth. It can be used for 3G or 2G networks as well.
- faster than TCP: Fast as there is not waiting.
- stateless: If the server dies and came back up. The client can continue to work without any interruptions. This is as there is not connection involved, which can get lost on server restart.

### Cons:
- No Acknowledgement: No ACK. There are chances of data loss.
- No Guaranteed delivery: as there is no ACK, might lead to daat loss. so, no guarantee. There is no retransmission as well.
- Connectionless: There is no physical connection. So, both client and server doesn't not know each other.
- No congestion control: When it receives the data, it just sends it. Adn it doesn't care if there is traffic or not. It does not wait for the traffic to get cleared.
- No ordered packets: It won't contain the headers to sequence or number the packets.
- Security: As we dont know who the client or server is. So, if the port is open, any one can send the data. Its risky.

## UDP Code Example (Nodejs and netcat(nc))
