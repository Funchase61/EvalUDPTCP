# Evaluation TCP/IP

## Table of contents

- [Introduction](#Introduction)
- [Manipulations list](#Manipulations-list)
- [Problems encountered and solutions](#Problems-encountered-and-solutions)
- [Theorical analysis](#Theorical-analysis)
- [Conclusion](#Conclusion)
- [Resources](#Resources)

## Introduction

<p>Welcome, everyone! Today, we're going to dive into a set of exercises focused on fundamental networking protocols: TCP/IP, UDP, and ARP requests. These protocols play a crucial role in communication across networks, from ensuring reliable data transmission to handling address resolution. Through these exercises, you'll gain a better understanding of how they function, their differences, and their practical applications. Let's get started! 
  
TLDR : you'll need to download Cisco Packet Tracer to do this exercices, and some VMs on Windows server iso</p>

## Manipulations list

### First exercice : creating our local network with working routing

So it's time to introduce ourselves to Cisco Packet Tracer ! Download it and open it.

When it's done, now we need to create our "simulation" of what is our network in our facility, so for that we'll need to install equipments. In our case we'll need to have 3 computer , 3 servers , 1 switch , 3 routers.

For Context : Our Facility have 3 anchors ( admin, technical, marketing ) each one have his own sub-network :

- Technical : 192.168.1.0/24
- Marketing : 192.168.2.0/24
- Admin : 192.168.3.0/24

each one have his own router and server to permits routing and Ip distribution by DHCP

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



## Problems encountered and solutions

## Theorical analysis

## Conclusion

## Resources
