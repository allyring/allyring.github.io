---
layout: post
title:  "Utilising computer networks in AV productions"
date:   2023-08-04 22:40:00 +0100
category: "AV"
---

Recently I've had the opportunity to integrate computer networking into some of the shows I've been working on for various different purposes, and I though it might be beneficial to technicians who haven't ever set up traditional computer networks to explain some key concepts and a few areas where computer networking can be used in a show.  
Additionally, I'll touch on the basics of how to optimise network performance, and how to keep your networks secure and reliable in order to keep your shows running smoothly.

*(Sidenote to any IT folks reading this, I'm oversimplifying in places here to try and make things a little clearer for folks without any IT experience or who are still learning; I'm aware that the rabbit-hole goes much, much deeper here!)*

---

# Uses of IP networking

I find it's always helpful to have a vauge idea of why a given topic is helpful before I start to learn it, so here's a few places where using IP networks can be helpful on a given show:
- Operating lighting fixtures using ArtNet or sACN.
- Utilising wireless networking to control digital audio mixers.
- Connecting digital mixers to stageboxes using protocols like Dante.
- Sending video over a network using RTP or other similar protocols.
- Troubleshooting a venue's internet connection.

I'll try and tie these in as I go, and once all of the prerequisite knowledge has been mentioned.

---

# Network Hardware
There are 7 main pieces of networking hardware that you might need to use on a show:
- Ethernet (sometimes called CAT5) cables
- Network Switches
- Wireless Access Points
- Network Interface Cards
- Routers
- Hardware Firewalls
- Servers

## Ethernet Cables
Ethernet cables - sometimes referred to as CAT5, CAT5e CAT6, or CAT cables - are the cables used to connect wired network devices together. They have rectangular ends with a shallow rectangular extrusion on one side where a small plastic clip is attached. When attaching a cable to an ethernet port (which is, as expected, more or less the same shape as the front of an ethernet cable), ensure that you hear the "click" of the clip locking the cable into place.

Note that the clip on an ethernet cable is fairly weak and is prone to breaking. If it breaks, the cable is much more likely to come loose after being plugged in, so try and avoid using cables with broken clips if possible.

These cables will come in a wide variety of lengths, from centimeters to tens of meters long.

For some situations, one connection between two devices will be enough, such as when connecting a stagebox to a digital audio mixer using a protocol like Dante, and this is one of the simplest (or even only) networks you might ever need to set up.

### Power over Ethernet (PoE)
Ethernet can also be used to deliver power to devices using Power over Ethernet (or PoE). This is quite similar to Phantom Power which is used to power condenser microphones, active DI boxes, or other equipment within the audio world.

PoE is "injected" into an ethernet cable using a PoE injector or other similar device.

Similarly to phantom power, Power over Ethernet has the ability to potentially damage equipment if used improperly, so it's worth double-checking to ensure that a device will be safe recieving it first.

## Network Switches
Network switches allow you to connect multiple wired devices to the same network. They offer 3 or typically more ethernet ports that you can connect devices to via an ethernet cable.

They work by keeping track of which devices are connected to what port, then when they recieve some data that's meant to reach a specific device, it forwards that data over the port that device is connected to <sup> [1]</sup>.

Older devices, called "hubs", performed a similar role, except they forwarded any data they recieved ont all connected devices, and expected those devices to figure out if that data was meant for them. 
Of course, this wasn't ideal for a secure network, as you could configure a connected computer to log any data it recieves regardless of whether it was meant for it or not.

Switches are essential pieces of hardware when creating larger networks.

## Network Interface Cards
Network Interface Cards - or NICs - are computer components that allow a computer to connect to a network.

There are two main types of network interface cards: wired and wireless.

Wired network interface cards are, as the name implies, used to connect to a wired network, and have an ethernet port to facilitate that connection. Most desktop computers come with a wired NIC built-in to the motherboard, but more can be added using additional expansion cards.

Wired NICs can also be found in USB to Ethernet adaptors, which are one of the few ways to get a wired connection on devices like tablets, phones, and modern laptops.

Wireless network interface cards (or WNICs) are used to connect to a network wirelessly via a Wireless Access Point. WNICs are often built into laptops, tablets, and mobile phones, and are sometimes present in desktop computers. Additionally, a WNIC can be added to a desktop computer using an expansion card, similarly to how additional wired NICs can be added.

## Wireless Access Points
Wireless Access Points - or WAPs (I'd advise avoiding that acronym) - are devices that allow other devices with a wireless NIC to connect to a network wirelessly. This communication is performed over Wi-Fi<sup> [2]</sup>.

One use of a wireless access point is to connect to a digital mixer wirelessly using a tablet and a mobile app. This allows engineers to adjust the mix from any point in the room.

## Routers
Routers allow different, smaller networks to be connected together to create a larger network of networks. They are typically used to connect a network to the internet.

Routers aren't typically used in live events (with the exception of live streaming), and are most often used for regular home or buisiness networks.

---

It's worth mentioning that the term router can refer to either a router in the traditional sense as described above, or the what internet service providers - such as British Telecom or Comcast - call the device that is given to you when you join a contract with them that gives you internet access.

These "routers" are actually often a combination of a wireless access point, a switch, and a router. Some people call them "gateways" or "modems", but I find that just calling them an _Insert ISP name_ router is clear enough in most cases. (I'll be refering to them as a quoted "router" for the rest of this blog post.)

This is obviously convenient for most consumers, since all three devices are in one place. However, this also means that unused "routers" can function as a makeshift switch, router, or wireless access point if budget or time is tight.

## Firewalls
Hardware Firewalls are devices that inspect network traffic passing through them against a set of rules and block or allow data to pass through depending on if they match or don't match these configured rules.

Software firewalls are configured on individual devices, and perform a similar purpose, except they only inspect traffic that is inbound or outgoing from that device.

Both hardware and software firewalls are a core component of ensureing that most networks are secure. However, for networks set up for live events, they can become problematic if forgotten about or if misconfigured. "Routers" also often have a simple firewall built in, which might cause issues if not reconfigured or disabled.

## Servers
A server is a computer on a network that runs server software. Typically, these computers are large devices that fit in a server rack, 
but any device that can run server software can be a server.

### Server Software
Server software is software that operates a service for users on a network, such as websites, email services, file transfer services, video streaming, etc.

# Network Statistics
The performance of a network is measured with a number of statistics.

## Bandwidth
Bandwidth refers to how much data can be sent over a network or connection within a network.

Bandwidth is sometimes referred to as "speed", and is measured in kilobits, megabits, or gigabits per second. A higher bandwidth is better.

Note that a kilobit is one eighth of a kilobyte, a megabit is one eighth of a megabyte, etc. If you need to estimate transfer speeds, then divide 8 by the kilobit, megabit, or gigabit value to get the amount of time it takes to transfer a kilobyte, megabyte, or gigabyte of data.

## Latency
Latency refers to the amount of time data takes to get from its source to its destination.

Latency is measured in milliseconds, and a lower latency is better.

## Packet Loss
Packet<sup> [3]</sup> loss is the number of transmission errors that occur during a communication.

The most common source of packet loss is two or more signals colliding on a given communication channel, such as on an ethernet cable, or on a Wi-Fi band. A lower packet loss is better.

# Wired vs Wireless networks
Wired and Wireless networks both have advantages and disadvantages, and it's important to consider when to use each one.

Wired networks are typically quite fast, reliable, and are slightly more resiliant to potential threats. This is due to the fact that physical connections between devices have a higher bandwidth, as well as lower latency and packet loss due to not having the overhead of needing to use Wi-Fi to send the packets over the air.

However, this comes at the cost of having devices on the network teathered to a single location or a small area. Running Ethernet may also be inconvenient in some situations, and less technical people may expect a Wi-Fi connection if they need network access.

Wireless networks, on the other hand, are, by comparison, slower and less reliable. This is due to the overhead that the Wi-Fi protocol implements to try and increase the reliability of wireless connections. The number of people in a room will also affect the reliability of a wireless connection as having many devices in a room can result in congestion.

However, wireless networks are much easier for less technical people to use, and can reduce on the amount of cable that needs to be ran. However, this ease of access can introduce an element of security risk to a network. Additionally, the ability to move anywhere in a room can be invaluable to certain applications like controlling a mixer remotely using a tablet.

Ultimately, they both have different use cases, and can be used in conjunction to achieve the goals of the network.

# IP Addresses and Ports
In order to send data between devices on a network, some sort of unique identifier for each device needs to be used. This unique identifier is called an IP address.

IP addresses are comprised of 4 numbers, ranging from 0 to 255, that are seperated by full stops (periods), such as `123.234.56.78` or `192.168.1.254`.

IP addreses can be manually set for each device, and this is a common practice when working with networks where new devices aren't being added to the network by users. Each device will have a different way to assign an IP address manually, so it's worth searching for tutorials on how to do so for each different device you use.

However, for a network where devices are being added regularly, or networks where less technical users are connecting with their own devices, IP addresses can be automatically assigned using a system called DHCP.

Networks won't usually have DHCP capabilities unless you set up a DHCP server<sup> [4]</sup>. However, many "routers" will have DHCP servers built-in, which you can use to quickly get automatic IP address assignment in a pinch.

---

A port is a number used to identify a service on a device. For example, if you have both a website being hosted on a device as well as an email service running on that same device, you would use different numbers to identify which service you want to interact with.

# Working with a venue's existing networking equipment
Most modern venues will have some networking equipment already, typically a wired network for all of the workstation computers in a venue, as well as 1 or more wireless access points so that guests, artists, and staff can get internet access from their phones.

Keeping this existing equipment in mind is important when setting up a network, as disrupting buisiness operations can be a quick way to get your new network shut down by management.

Make sure that your wireless access point names don't overlap with existing names in use by the venue, and try to keep the two networks separated if possible.

---

If you're lucky, you'll find (hopefully labelled or numbered) ethernet patch points near standard 13A power outlets or near other patch points in a venue. These points likely connect to a patch panel in a server cabinet somewhere in the venue, and it's worth making use of these existing, long distance connections between different parts of the venue when designing a network.

There may also be ethernet connections at audio or lighting patch points, which are typically set up to go to set locations that might be helpful for a show network.

### Patch Panels
Patch panels are used as a central point where individual ethernet patch points in a network centrally converge.
By using short ethernet cables - sometimes called patch cables - you can easily connect two patch points together by connecting the patch cable to the two ports on the patch panel with the same labels as the patch points in the venue that you want to connect.

# Network Security
Network security in a more broad sense is a topic with a lot of depth to it, but in the case of a network for a show, it's often a bit simpler to handle, since preventing initial unauthorised access from occuring is often much easier than on a more traditional network.

The easiest way to prevent someone unauthorised from accessing network devices is to remove the 3 main ways that someone could access your network, which are through wired access, wireless access, and internet access.

## Securing Wireless Access
Wireless access is the most likely way that someone could gain access to your network, due the the fact that most people carry phones on them. Ensuring that your wireless access point is configured to use a strong authentication protocol, like WPA3, with a strong password is the easiest way to secure a wireless network. Additionally, implementing device "allow lists" to a wireless access point can be an effective technique to implement if your wireless access point allows for it.

## Securing Wired Access
The easiest way to secure wired access it to ensure that all ethernet cables and other pieces of network hardware, such as switches, are out of the way of anyone who isn't authorised to access the network.

If you can't put a device in a secure place, then it's best to have at least a decent line of sight to it, so that you can spot if someone is trying to do anything suspicious.

In practice however, very few people will try and intentionally break a wired network. If anything, having cables that aren't out of the way is more of a health and safety hazard.

## Securing Internet Access
There are very few situations in which a network for a show needs internet access, so the simplist way to mitigate the risk of unauthorised access via the internet is to simply not connect the show network to the internet. This process is called "air-gapping" a network.

However, installing software from the internet using a USB drive, or using a device that has previously been connected to the internet without a factory reset can sill present a risk. Ideally, you should be using specific devices that haven't accessed the internet and have had all installed software scanned with antivirus tools. If this isn't possible, keeping all software (including the operating system) up-to-date, preforming regular malware scans, and and generally being sensible on the internet is the best way to mitigate risk.

Additionally, configuring a hardware firewall to only allow necessary  connections between the internet and your network is an additional measure that can be taken to secure your network.

# Network Reliability
Often a concern when setting up any technology for an event is "will it be reliable?". Setting up a network is no different. Complex networks contain a lot of elements that can be unreliable if not considered when designing the network.

## Bandwidth Issues
One way that a network may become unreliable is due to insufficient bandwidth or a high degree of packet loss on a network. This can occur when multiple spearate services are trying to utilise the same connection pathways on a network, such as the trying to transmit on the same ethernet cable or sharing a switch.
The easiest way to reduce the congestion or packet loss in this case is to separate individual services or groups of services that don't need to communicate onto one or more separate networks. This will reduce the amount of data collisions during transmission, and will increase the available bandwidth for each service.

## Wireless Issues
Another way that a network may become unreliable is if there is a wireless section of a network. Wireless networks, as mentioned previously, are a bit less reliable than a standard wired network due to the overhead of having to communicate over radio frequencies. 

There are many factors that can impact the reliability of a wireless network, such as distance from the wireless access point, and traditional bandwidth issues. 

Distance issues can be addressed by adding additional wireless access points to a network, or by positioning an existing wireless access point away from walls or other surfaces that may block radio frequencies.

Bandwidth issues can be addressed in similar ways to wired networks, but a more effective way to handle separating a network up is to split groups of users up into different networks with their own wireless access point. 

# Setting up a simple "AV" network
When setting up a network, I find it helpful to break the process down into 6 stages:

1. Determining the use cases of the network
2. Researching into the technologies that those uses require
3. Developing a plan while considering reliability and security
4. Deploying the hardware
5. Configuring the software
6. Testing and troubleshooting the network

For each stage of the process, information about a hypothetical example network will be included to better illustrate each step.

## Determining use cases
Before making any design decisions, it's important to consider what the actual purpose or purposes of the network are.

Sometimes this job will be done for you by other technicians or members of staff who have specific requirements, but if you're the only technician, you may need to do this yourself.

For this example network, the goal will be to allow a digital mixer to be controlled remotely with a tablet, and to send a camera feed from the room the performance is happening in to other locations in the building, such as a bar and a staff room.

## Researching into technologies
Now that the uses of the network have been defined, you can start to figure out how you'll implement those uses. This will typically involve a bit of research to find out what technologies exist for the uses that have been specified.

Some good sources of information include documentation from any hardware manufacturers, software or protocol documentation, opinions or reccomendations from other technicians, tutorial blogs or videos, and other sources on the internet.

For the example network, a technician would need to look into technologies to send video over a network, how to recieve and display that video, and how to connect their digital audio mixer to the companion mobile or tablet app.

For these uses, a technician might read the documentation for their mixer, watch tutorials, or read guides on how to connect to the digital mixer. They might also look into reccomendations on sending video over a network, and discover technologies like NDI and RTP. 

Its worth knowing that there often isn't one "correct" way to implement a solution, but that different solutions will have different pros and cons. In this case, NDI is simpler to set up, but is a bit less reliable, so a technician may choose to use RTP instead.

## Developing a plan
Now that you know what technologies you'll be using, it's time to consider what hardware devices and software products will be needed.

While this process can be done on the fly as the network is being set up, if you're inexperienced - or even if you're experienced and have some time - it's worth doing more formally.

Some things that should be noted include:
- How many devices will be on the network?
- What other devices does each device need to communicate with?
- What will each device need to directly connected to to facilitate these connections?

- How much data will be sent over this network? 
- Will this network need to use wireless devices?

- How far do I need to run any Ethernet cables?
- Are there any PoE requirements on this network?
- What patch points (if any) have been connected together at a patch bay?

- What IP addresses will each device have?
- Does DHCP (automatic IP address assignment) need to be set up?

---

It may also be helpful to draw a quick diagram of what devices need to be connected together, called a network diagram.

Writing some brief notes on these sorts of question will help you figure out what hardware you need to connect everything together

Writing brief notes will also help you restore a venue's network back to normal once the show's over if the new network is temporary, and more detailed notes will help you troubleshoot and upgrade a more permanent network.

At the least, I'd reccommend drawing a simple network diagram, writing down a list of what hardware you'll need, and noting down a few important IP addresses once they've been assigned to devices.

### Considering Security
The security of the network is always an important consideration when planning a network out. A few questions that you should ask include:
- Does this network need to connect to the main buisiness network?
- If so, what buisiness network services are strictly needed?
- Does this network need to connect to the internet?
- If so, what internet services are strictly needed?
- How could someone unauthorised initially connect to this network?

The answers to these questions should determine what security measures you should use. The ones detailed in the previous section on security should be a good starting point.

For the example network, nothing needs to be connected to the internet or to the main buisiness network, which rules out a lot of risks.
Additionally, there are 3 main ways someone could access the network: through an unsupervised switch or patch point, through an unsupervised computer, or through a wireless access point.

The first two ways the network could be accessed are fairly easy to address by ensuring that any unused patch points are disconnected for the duration of the event, and that there's always at least a relaible line of sight to any switches, computers, or other hardware. Ideally, these devices will be inaccessable to guests, but this isn't always possible.

The wireless access point, however, will need to be secured using a strong password and authentication protocol.

### Considering Reliability
A network for an event like a show needs to be reliable. A few questions  you should ask yourself include:
- Can I separate this network out into multiple smaller networks?
- Could the services on this network exceed the available bandwidth?
- Where will any wireless access points be placed to ensure that devices have a reliable connection to it?
- Is wireless access needed in multiple rooms?

Again, following the advice detailed in the previous section on network reliability should be a strong starting point.

In the case of the example network, the two systems in use - digital mixer control and video streaming - don't really need to interact, so the network can be split into 2 separate networks, one for each purpose.

Video streaming, notably, can be quite network-intensive as it requires a high bandwidth. Thankfully, on a local network, this shouldn't be an issue for one or two clients for one video feed.

The wireless access point, in this case, might be placed at a slightly elevated, more central position in the room to allow the audio engineer to use the tablet from anywhere in the room. However, the only room wireless access is needed in will be the room the event takes place in.

## Hardware Deployment
Once you have an idea of what hardware you'll need, you can start running ethernet cables, connecting patch points together, putting switches where they need to be, and generally getting all of the hardware on the network in place.

Whilst you're setting hardware up, ensure that any ethernet cables you connect are secure, avoiding cables that don't have a clip.

You'll also want to ensure that each device on the network is getting power, whether that's from a wall socket or PoE.

For the example network, a "router" which would be used as a wireless access point was connected directly to the digital mixer over an ethernet cable. The video camera was then connected to a laptop, and the laptop was connected by an ethernet cable to a patch point in the wall.

That patch point was then connected to a switch at the patch bay, which was then connected to ports on the patch bay that corresponded to patch points near the TVs that the video was meant to stream to.

At those patch points, I then put a computer near each one which I then connected to the patch point using an ethernet cable. I then connected the computers up to the TVs that were to play the video stream back.

## Software Configuration
Once all of the hardware is in place and devices are starting to be powered on, you can begin to configure each device as needed.

Ensure you configure IP addresses manually for each device if you aren't using DHCP.

In the case of the example network, the "router" connected to the mixer would need to have DHCP disabled, and the Wireless Access Point reconfigured to require authentication and to have a stronger password.

### Configuring "Routers" for different tasks.
The only configuration you might need to do on a "router" is disabling the built-in DHCP server in order to prevent IP addresses being assigned automatically if you plan on assigning them yourself, or disabling the wireless access point if you plan on using it solely as a switch and/or DHCP server.

However, if you need automatic IP assignment for a part of or all your network, then connecting a "router" to the network is a quick way to do so.

Additionally, if the default Wi-Fi password is insecure, you may need to change it to something more secure.

## Testing and Troubleshooting
The last step is to test the network to see if everything that's been implemented works. Hopefully it'll work without needing to reconfigure or troubleshoot anything, but if things aren't working, then there are some techniques and tools you can use to see where things are going wrong.

### Troubleshooting networks with the command line
For troubleshooting networks, it's helpful to have access to the command prompt on Windows or the terminal on MacOS and Linux in order to run commands that will give you more information.

The `ping` command on all platforms will perform a simple test to see if you can reach a given device on a network. The syntax for `ping` is `ping target.ip.address`. 

`tracert` on Windows and `traceroute` on MacOS and Linux will show you additional information about the journey between two devices, but not all devices will show up, such as switches. It's more helpful when determining the route between two networks on the internet.

To show information about the network interface cards on a device, run `ipconfig` on Windows or `ifconfig` on MacOS or Linux to display information about the configuration of connected network interface cards.

### Troubleshooting through signal flow
The easiest way to troubleshoot a network is to follow the path that a signal should travel, checking for issues as you go. Here, a network diagram can be quite helpful in determining what devices you need to check in what order.

---

Starting with the device where functionality is being tested, ensure that each device has power, has a functional network interface card, and that the connection to that network interface card is secure. Most network interface cards have some indicator LEDs that you can use to check if data is being transmitted.

Then, check the various configuration options on that device; this includes checking the IP address using `ifconfig` or `ipconfig` to see if it has been set correctly by either yourself or automatically by DHCP. If you're expecting the IP to have been configured by DHCP but it hasn't been set, then start following the signal as it would head to the DHCP server.

Next, check the ethernet cable connecting the two devices together; if you're able to, try replacing the cable with a cable that's known to be working fine.

After that, check the next device in the signal path, and keep going until you have found a fault. Once that's done, check the functionality on the original device again. If it's still not fixed, then start again from that device and keep gpoing further down the signal path.

# Conclusion
While this post has outlined a lot of the very basics of networking, it's wroth digging deeper into the technical aspects of the topic, especially if you're planning on creating a more permanent or regular network for your shows.

It's also worth noting that I didn't include much specific instruction on how to configure devices, since every device will be configured slightly differently. I'd strongly reccomend using a search engine like Google to find specific instructions for each device you're using.

# Extra Notes:
Damn, that was a long read. Hope that was interesting! Here are a few extra notes on some of the stuff I simplified that might be helpful if you're like me and find that black boxes aren't satisfying answers:

## #1: How a Switch Works in more depth
Switches track the device that's connected to each port by getting the MAC address and IP address of that device. A MAC (or media access control) address is a unique, unchanging identifier for a device that is set by the NIC manufacturer. A MAC address is sometimes called the "physical" address of a device, wheras an IP address is considered a "logical" address.

## #2: Disambiguation of Wi-Fi
Wi-Fi is actually the name of the family of protocols that are used to connect to a network wirelessly. Wi-Fi is a trademark of the Wi-Fi alliance. The protocols are members of the IEEE 802 protocol family. Since the specific names are usually quite a mouthful, most people refer to the specific protocol they're using as "Wi-Fi".

## #3: What's a packet?
A packet is a small piece of data that is sent over a network. Larger sections of data are split up into smaller packets, such as smaller chunks of an audio file, and are then transmitted over the network.

## #4: DHCP servers
Dynamic Host Configuration Protocol (DHCP) is a protocol used to configure IP adresses on new devices on the network. When a new device connects without an IP assigned manually, the device will query the network to see if there is a DHCP server available. If there is, the device requests an available IP address from the DHCP server, and starts to use that IP address.