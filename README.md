# Evaluation TCP/IP
___________________
## Table of contents
_______________________
- [Introduction](#Introduction)
- [Manipulations list](#Manipulations-list)
- [Problems encountered and solutions](#Problems-encountered-and-solutions)
- [Theorical analysis](#Theorical-analysis)
- [Conclusion](#Conclusion)
- [QUIZZZZZ](#QUIZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZ)

## Introduction
_____________________________
<p>Welcome, everyone! Today, we're going to dive into a set of exercises focused on fundamental networking protocols: TCP/IP, UDP, and ARP requests. These protocols play a crucial role in communication across networks, from ensuring reliable data transmission to handling address resolution. Through these exercises, you'll gain a better understanding of how they function, their differences, and their practical applications. Let's get started! 
  
TLDR : you'll need to download Cisco Packet Tracer to do this exercices, and some VMs on Windows server iso</p>

## Manipulations list
_________________________________
### First exercice : creating our local network with working routing

So it's time to introduce ourselves to Cisco Packet Tracer ! Download it and open it.

When it's done, now we need to create our "simulation" of what is our network in our facility, so for that we'll need to install equipments. In our case we'll need to have 3 computer , 3 servers , 1 switch , 3 routers.

For Context : Our Facility have 3 anchors ( admin, technical, marketing ) each one have his own sub-network :

- Technical : 192.168.1.0/24
- Marketing : 192.168.2.0/24
- Admin : 192.168.3.0/24

Each one have his own router and server to permits routing and Ip distribution by DHCP

### 1. First step : 

Create our work environment, it should look like this : 

![First step exemple](https://i.imgur.com/erDbxTN.png)

So as we can see each one of our anchors are there with their servers and routers, name them properly to have a better insight of what you're doing.

Now what do we do with this ? time to allocate ips to our subnets

### 2. Second step :

Allocate a static ip to our Servers, it will always be 192.168.x.10 , in x will be change by our subnet numbers as TECH will be 1.10 , Marketing will be 2.10 and Admin will be 3.10

To do it you'll need to proceed like that in each server :

![exemple of allocating](https://i.imgur.com/9mxRtBE.png)

As you can see we're adding our ip, our subnet 255.255.255.0 ( /24 ) and our Gateway.

The gateway is basicaly the ip of one of our net interface on our router, its needed for our subnet to interact to another subnet via our router ( which is doing his job of routing our packets ). And that's what we will configurate now.

### 3. Third step :

Now let's configure our routers, we need to configure each net interface of them. One being the gateway of our subnets and the other being the one needed for them to communicate with each other :

1. First interface

![exemple of router](https://i.imgur.com/sGEEkno.png)

2. Second interface

![exemple of router](https://imgur.com/WIVQYv6.png)

You need to do that for each one of them where you replace 192.168.x.1 / 10.0.0.x , x by the number of others subnets.

Now time to make our Routes between them :

Here you'll need to precise which subnet you aim and how to access it for exemple :

Here we configurate Tech router :

- Network : 192.168.2.0
- Mask : 255.255.255.0
- Next Hop : 10.0.0.2

But our subnet 1.0 need to also communicate with 3.0 so you need to create a second rules of routing : 

- Network : 192.168.3.0
- Mask : 255.255.255.0
- Next Hop : 10.0.0.3

Exemples of each routers to add clarity :

![exemple of router](https://imgur.com/uXG3Vf7.png)

![exemple of router](https://imgur.com/ILpwVSj.png)

![exemple of router](https://imgur.com/UFo9YF1.png)

With this our routers and routing service should be operating ! nice work :)

### 4. Fourth step :

Now time for DHCP to work ! 

Before going on our servers to configure that we will check if our DHCP protocol on our clients is activated, to do so you need to click on the equipment and check in Desktop > Ipconfiguration :

![exemple of material](https://imgur.com/Z6lFuZS.png)

Do that for each one of them to be sure that DHCP is used on every client.

Now our server need to be configurated to give an ip adress to our clients automaticaly

To do so we will need to activate the Dhcp gestion protocol and create ranges of ip for each of our subnets :

- Activate the service by clicking on ON
- Give it a name
- Default gateway so ( 192.168.x.1 )
- DNS Server if internet is needed ( 8.8.8.8 )
- Add a starting ip of your allocating range
- Subnet mask like always ( /24 )
- How much ip adress you need to be allocated automaticaly in your range so here if we say 100 and start at 192.168.1.100 it will automaticaly add from .100 to .200

The last two parameters can be ignored then ADD the rule.

![exemple of material](https://imgur.com/eoeCQJ2.png)

Do this for every servers where like every other step before changing x by the number associated to our subnet.

Well played, Now our network should be operating correctly time to test it.

### 5. Last step :

First we will check if our clients did get correctly an ip in our range configurated.

To do so lets check with a command prompt in our client choosen. Click on one of your client then Desktop > command Prompt

Then proceed to use the command `ìpconfig`

![image](https://github.com/user-attachments/assets/dbe39d3d-e8f1-4336-a9d5-d145a215be16)

Nice it seems that it worked ! so now let's ping clients with this one showed in the exemple in another subnets by using the command `ping 192.168.x.100`

![image](https://github.com/user-attachments/assets/16bb6411-77f7-47f8-8940-7d8542ffe0b8)

we see that packets coming back from the clients we did ping so our network is clearly working well !

It's enough for our first exercice. it showed you how to create a simple network and make communication via routes

### Second exercice : understanding UDP and TCP by testing them

For this exercice you will need to install wireshark but everything else can be done directly on your desktop

The objective here is to understand what's the difference between Udp packets and Tcp ones, is one performing better than the other or do they just do something different ? Let's see this.

First open up Wireshark and now select the interface you want to survey ( for me it's my wifi so it's ginerally starting with a W but if it was via ethernet it's E ) : 

![image](https://github.com/user-attachments/assets/44d9091c-5874-4172-a920-cadf6f09221d)

Double click on it, it should open up the survey window.

Now we want to test each one of them : 

- TCP via this filter `tcp.port == 80` ( 80 is the port for http )
- UDP via this filter `udp.port == 53` ( 53 to test on twitch for exemple in streaming )

To test TCP after using the filter above, just go on your browser and search anything

You should have something like that appearing on your wireshark : 

![image](https://github.com/user-attachments/assets/19d49080-4d86-446f-86c9-8a53f15ef9e0)

Now do the same on another window of wireshark but for udp, to test udp open a stream on twitch for exemple and see packets flowing on your survey :

![image](https://github.com/user-attachments/assets/021559cc-7376-427b-9bb4-184c0c26a9c5)

Those are DNS request cause we're asking google to give use "autorisation" to have access to it but you can see than every request is using UDP protocol on port 53 ( USER DATAGRAM PROTOCOL ).

Now if we compare them we can see that UDP is much faster than TCP see with the parameter that is highlighted : 

![image](https://github.com/user-attachments/assets/0c70d6d1-81c3-4754-bf33-29119e2cf468)

But it has a cost for that speed for udp. if a packet is lost it cannot be found anymore and the flow will still continue without interruption.

For Tcp it's different, it is a bit slower yes. But TCP is capable of retransmiting packets that couldn't his way, for exemple here a restranmitted packet : 

![image](https://github.com/user-attachments/assets/2711581c-682c-4db4-8be5-0c049378b7e0)

Im doing this on a stable connection right now so it's pretty rare to have packet loss but if we could simulate a connection problem in any way those packets would have been retransmited by TCP, with UDP you would have lost your frames on the stream you were watching or anything that is in live.

### Third exercice : Simulating TCP and UDP on Cisco Packet Tracer

So here we want to simulate what we were talking about on the exercice 2, we shall create 2 network to have a better view on each case :

Just create a basic network with a connection between a server and a client but each of them have their router, copy paste it to have a second simulation.

![image](https://github.com/user-attachments/assets/cfea2750-dc85-482c-ba03-5b13d54c2c83)

Let's simulate our TCP by just opening up a browser and interact with the server on our client :

- Open up your browser in your client : Desktop > Browser
- Tap `http://192.168.2.10` ( depends on what ip you did allocate to your server )
- proceed to see a simulation using the simulation window on the right and filtrate HTTP

Exemple here :

![image](https://github.com/user-attachments/assets/a72cda4c-307a-4694-9b2a-d43370212f8b)

![image](https://github.com/user-attachments/assets/fad3ccde-5104-45c1-86a0-f4c08662cd4c)

![image](https://github.com/user-attachments/assets/c49e246f-a39c-4222-96c3-e5ea41874db7)

We see that it worked just fine by showing us the simulated website of our server, it takes 0.13 sec for a packet tcp to go to the server and return back to us 

Now for Udp let's use a traffic generator on our client : 

Prepare the packet and where it shall be sent by fill the parameters correctly ( see the exemple ) then send it and simulate ( with the right filter Udp here ) :

![image](https://github.com/user-attachments/assets/13c2761e-0894-4357-95aa-deec05e5879f)

![image](https://github.com/user-attachments/assets/277b7878-c2a7-4b8a-a828-bb1ff4ed551a)

We see that it worked just fine like the tcp packet but here it takes 0.006 sec to go to the server and return back.

It means that an udp packet is 200 Times faster than a tcp packet

Now why shall we use one or another :

Choosing between TCP (Transmission Control Protocol) and UDP (User Datagram Protocol) depends on the requirements of the application and the type of data being transmitted.

If data accuracy and reliability matter → Use TCP.

If speed and low latency are more important → Use UDP.

### Last exercice : ICMP and ARP

Deeper understanding on what is really ICMP and ARP, because at first we're using a lot the command `ping` but what is it really ? well it's literally a request ICMP 

So we were technically already using this kind of request to test connection between server/client, a ping can tell you a lot of thing important. like if there is a problem in your network installation. if a ping can't return back there is a problem.

Let's a see an exemple of what is happening when a ping can't return back : 

![image](https://github.com/user-attachments/assets/608ad2ea-1b61-4385-94a1-06c16c0c577e)

Here we can see that the packet sent is lost. it indicates us that there is no receiver found.

So maybe a problem in our network ;)

Now we can try to see if we can solve this problem with the request ARP, a request arp is made for searching an user with the correct ID so here an adress MAC

Why do we need this ? well it is used to be sure that the packet u send isn't sent to everyone and create an exception to who shall accept that packet.

By using the command `arp -a` we can see if the ip aimed got an adress MAC :

![image](https://github.com/user-attachments/assets/d60478a8-fde1-4bbc-b65d-990566daf773)

It seems that it isn't recognised as a Mac adress detainer.

So basicly the server don't exist ( it was made on purpose cause we didn't create any server with that ip on that simulation of TCP/UDP )


## Problems encountered and solutions
_________________________________
If you follow the exercices word by word normaly there isn't any problem, some problem would have occured with VMs but we didn't use it here.

## Theorical analysis
_________________________________
### 1. Importance of TCP/IP Protocols in Modern Networks

The TCP/IP (Transmission Control Protocol / Internet Protocol) model is the foundation of communication on the internet and private networks. Its importance lies in several key aspects:

    - Network Interconnection: Enables devices to communicate regardless of their architecture or operating system.
    
    - Standardization: Universally used, ensuring interoperability between different hardware and software systems.
    
    - Modularity: Composed of multiple layers (Application, Transport, Internet, Network Access) that allow for efficient and adaptable data transmission.
    
    - Scalability: Supports small local networks as well as global-scale communications

### 2. Advantages of Different Protocols (TCP vs. UDP)

|Feature	|TCP (Transmission Control Protocol)|UDP (User Datagram Protocol)|
|---------|------------------------------------|---------------------------|
|Connection|Connection-oriented (3-way handshake)|Connectionless|
|Reliability|	Ensures data delivery (error checking, retransmission)|No guarantee of delivery|
|Speed	|Slower due to overhead	|Faster due to minimal processing|
|Ordering	|Maintains packet order|	No sequence control|
|Use Cases	|Web browsing (HTTP/HTTPS), file transfer (FTP), email (SMTP, IMAP, POP3)|	Streaming, gaming, VoIP, DNS, IoT applications|

TCP is ideal when data integrity and reliability are crucial, whereas UDP is preferred for speed and low latency applications.

### 3. Observations Using Wireshark and Simulators

Wireshark, a network packet analyzer, allows us to observe and analyze network traffic in real-time. Some key observations include:

    - TCP Handshake: When initiating a connection, TCP uses a three-way handshake (SYN → SYN-ACK → ACK). This ensures a stable connection before data transmission begins.
    
    - Retransmissions: If a TCP segment is lost, Wireshark captures retransmission attempts, confirming TCP’s reliability.
    
    - UDP Simplicity: UDP packets are visible as independent datagrams without acknowledgment, illustrating its connectionless nature.
    
    - ARP Requests: Wireshark can capture ARP requests and responses, showing how MAC addresses are resolved from IP addresses in local networks.
    
    - Packet Loss in UDP: When using UDP-based applications (e.g., video streaming), dropped packets may appear without retransmission, highlighting its trade-off between speed and reliability.

Using network simulators (e.g., Cisco Packet Tracer, GNS3), we can recreate network scenarios to test protocol behavior under different conditions.

## Conclusion
___________________________________
The TCP/IP suite remains the backbone of modern networking, enabling reliable and scalable communication. TCP ensures data integrity for critical applications, while UDP provides fast and efficient transmission for real-time services. Tools like Wireshark and network simulators help visualize and understand how these protocols function in real-world scenarios.




# QUIZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZ
_______________________________________________

1. A
2. C
3. C
4. C
5. B
