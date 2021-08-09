\newpage

**Executive Summary**

Placeholder text: When people think of Internet performance it is typically solely in terms of speed. But when considering the end user quality of experience (QoE), latency is usually a key factor. Unfortunately, latency is not well understood in the policy or regulatory spheres or by the average consumer. This paper will explain and explore latency, including idle latency, latency under load, components of latency performance, how latency varies by access technology and/or protocol, and how latency can affect the user QoE for certain types of applications such as video conferencing, remote learning, gaming and more. As well, while the FCC MBA program reports on latency it takes no position on what is good or bad latency - a question this paper will explore. Additionally, this paper will examine metrics for characterizing latency performance, including various summary statistics for latency and latency variation (often referred to as "jitter"). Finally, the paper will look to the current state of the art and the future to explain Active Queue Management (AQM) and other low latency services - and the new types of applications that this may enable in the future.

\pagebreak


# Introduction (3pgs?) (Jason)
* Effect on Applications
	* User experience and lag in web browsing, web conferencing, and gaming 
	* How latency impacts UX 
* Mention simple test for buffer bloat / lag
* Trends of User Experience and Network Performance
* Why latency matters
* Common misconceptions 


# Definition of Latency 

A good starting point is to first understand idle latency, which reflects the underlying and inherent latency of an end-to-end path, because working latency simply builds on top of that. The latency between a laptop and the next hop of a packet on the LAN will typically be quite short. As you add successive network hops from the laptop to the home gateway, then the ISP network, and all the way to the destination server, the latency will increase as each new link in the chain is added. But the number of links in and of itself does not necessarily mean greater latency per se.

This is because some links can be quite short, such as a fiber link between a data center switch and a destination server, while other links can be long, such as an east-west fiber link across the U.S. And physical media such as fiber is limited by the physics of the speed of light. This means that, generally speaking, latency increased as distance increases: the time to send a packet across town will be less that the time to send a packet across the country. 

Latency also varies by different types of physical media or type of network (e.g., type of ISP access network technology). For example, looking at the physical media, a wired gigabit fiber connection will typically latency that a copper-based Fast Ethernet connection. In addition, the latency properties of ISP access network technologies will also cause latency to vary – such as the difference between FTTH, DSL, HFC, Wi-Fi, 5G, geostationary satellite, low Earth orbit (LEO) satellite, etc. As a result, idle latency tests of an end-to-end path will simply reflect (1) distance and (2) underlying network technologies. This is the baseline latency that is the starting point for understanding real world end user performance.

However, latency also varies, and significantly so, based on underlying network conditions. That means that latency may increase as traffic volume increases or as the capacity of a connection fills up. It can also mean that latency varies as a result of a mix of different kinds of traffic on the network (e.g., bulk downloads and online game play). When adding in real traffic of any type and volume, we can then see how the network reacts under real-world conditions and understand the so-called working latency of the path.

Finally, since latency is a measure of delay, it is a measure of time. Network latency is typically described in milliseconds of time, though it can often grow to several seconds of time. While idle latency seeks to measure the baseline latency performance of a given end-to-end path, working latency measures the latency performance under real-world conditions when a user’s connection is utilized to a normal extent. 

(missing: Impact that data rate has on latency?)
![image](https://user-images.githubusercontent.com/8984861/128738550-b73732e2-a856-4a14-ae25-0ed223f72884.png)



# Sources/Contributors to Latency (8pgs)

## Link technologies in place along the path (3pgs)
* Ethernet, (Greg)
* WiFi, (DaveT)
* DOCSIS,  (Greg, Jason)
* DSL, (Barbara)
* PON, (Barbara)
* LTE/5G, (Barbara)
* PLT (Barbara)
* Satellite (DaveT)
	* media access, scheduling, etc.
* Core network links (Shamim)
	* Propagation velocity, path distance, path stretch

## Buffering delays (3pgs)
### Impact that senders & network protocols have on path latency (author?)
* Congestion Control (Classic TCP, BBR, TCP Prague, Delay-Based, Real-Time, LEDBAT)
* Other “bursty” applications

### Queuing implementations (Dave Taht, Greg)
* FIFOs (buffer sizing)
* AQMs: CoDel, PIE, DOCSIS-PIE, Cobalt, etc.
* Flow queuing: fq\_codel, fq\_pie, CAKE

## Latency contributions from endpoints (client & server) (1pg) (Dave Taht, +coauthor)
* Socket buffering & Offloads
* Head of line blocking & retransmissions
* Server resource contention
* VMs/Containers 

## VPNs and Proxied Paths (1pg) (Barbara)
* iCloud/Safari Private Relay, etc.


# Current and Future Technologies to improve latency performance (3pgs) 
* Recommendations for networks and applications	(author?)
* Migration to the network edge (CDNs, MEC) (Shamim)
* Local caching (DNS, etc.) (author?)
* Traditional Quality of Service differentiation (Greg)
* Explicit Congestion Notification (Greg)
* QB/NQB distinction & Low-Latency, Low-Loss, Scalable throughput architecture (Greg)


# Metrics and methods for characterizing latency performance (5pgs)

## Latency for a path in a live network is variable – a statistical distribution (Greg)
* Descriptive statistics: Average latency, min/mean/max/std
* Order statistics (P0,P99,P99.9)
* Packet Delay Variation vs. Inter-Packet Delay Variation vs. “Jitter”
* CDFs
* "Responsiveness" RPM (round-trips per minute)

## Measuring latency (author?)
* Protocols: ICMP (PING), UDP (STAMP, TWAMP, SamKnows, etc), TCP, HTTP/2 Echo
* Awareness/manipulation of conditions (i.e., monitoring cross-traffic, introducing cross-traffic, etc.) 
	* Idle latency
	* Working latency
		* Latency Under Load


# How do latency and latency variation impact user experience? (incl. mention of latency mitigation techniques) (5pgs)

## VoIP, video conferencing (Cullen, Dave Taht)
* Mean Opinion Score
* Impact to the call not feeling interactive 
* User "talking over" other users 
* Jitter buffers and how jitter increases microphone to speaker latency 

## Multiplayer online gaming (Alex)
* Lag compensation techniques
* Jitter buffers

## Cloud gaming (Alex)
* Controller-to-photon latency 

## Web browsing (Peter)
* Page Load Time & other metrics

## Future applications
### Autonomous cars (Barbara)
### Cloud VR (Greg)
### Remote Surgery (Dave Taht)


# Conclusions & Recommendations


\pagebreak
# Heading Level 1

aldkfja;dklfja;dklfja;ldkfja

example of reference [@aca1]

Example footnote marker [^1]

Example footnote text 

[^1]: Put the text for footnote here


## Heading Level 2
### Heading Level 3
