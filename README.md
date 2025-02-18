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

Allocate a static ip to our Servers, it will always be 192.168.x.10 , in x will be change by our subnet numbers as TECH will be 1.10 , Marketing will be 2.10 and Admin will be 3.10

To do it you'll need to proceed like that in each server :

![exemple of allocating](https://i.imgur.com/9mxRtBE.png)

As you can see we're adding our ip, our subnet 255.255.255.0 ( /24 ) and our Gateway.

The gateway is basicaly the ip of one of our net interface on our router, its needed for our subnet to interact to another subnet via our router ( which is doing his job of routing our packets ). And that's what we will configurate now.

Now let's configure our routers, we need to configure each net interface of them. One being the gateway of our subnets and the other being the one needed for them to communicate with each other :

1. First interface

![exemple of router](https://i.imgur.com/sGEEkno.png)

2. Second interface

![exemple of router](https://imgur.com/WIVQYv6.png)

You need to do that for each one of them where you replace 192.168.x.1 / 10.0.0.x , x by the number of others subnets.










## Problems encountered and solutions

## Theorical analysis

## Conclusion

## Resources
