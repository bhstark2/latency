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


# Definition of Latency (1pg) (DavidReed, Jason)
* Latency as a property of a network path (0.5pgs)
	* Points A & B, and the network path that connects them
	* Latency is additive along a path – contributions from multiple sources
* Latency as a property of a network condition (0.5pgs)
	* Idle latency vs Working latency
* Impact that data rate has on latency


# Sources/Contributors to Latency (8pgs)

## Link technologies in place along the path (3pgs)

### Ethernet, (Greg)

### WiFi, (DaveT)

### Powerline Carrier (PLC) (Barbara)

### DOCSIS,  (Greg, Jason)
 
### Digital Subscriber Line (DSL) and G.fast

ADSL, ADSL2+, VDSL, VDSL2, and G.fast are standards defined by ITU-T. DSL technologies run over twisted-pair copper wires. The older Asymmetric Digital Subscriber Line (ADSL) technology was generally used on loop lengths of 1 mile or less. The newer ADSL2+, Very high-speed DSL (VDSL) and VDSL2 technologies are generally used on shorter loops in a fiber to the node (FTTN) configuration (with copper to a node and fiber from the node to the central office). Since copper is a very efficient transmission medium, the time for a signal to travel these distances is very small and does not contribute significantly to latency. Encoding and decoding DSL signals does add some small latency. But this delay is also very small.

Some ADSL2+ and VDSL deployments used a technique known as interleaving to be more resilient against noise on the line. Noise can result in lost bits of data. With interleaving, it is sometimes possible for the ADSL2+ or VDSL receiver to recover these lost bits. If lost bits are not recovered, the loss can result in either missing information (e.g., clipped sound in an audio transmission) or cause the data to be retransmitted (which causes delay). But interleaving adds delay to accomplish this resiliency. Common interleaving delays range from 2 - 10 ms, when it is enabled.

G.fast is another copper-based technology (over twisted pair or coax) that can be used on very short loops (up to around 500 ft). Interleaving is not used with G.fast and the loop length and encoding mechanisms add small latency.

When interleaving is not used, latency of these technologies tends to be measured in nanoseconds.

### PON, (Barbara)


### LTE/5G, (Barbara)

### Satellite (DaveT)
	* media access, scheduling, etc.
	
### Core network links (Shamim)
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
