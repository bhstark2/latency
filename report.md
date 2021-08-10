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
* Ethernet, (Greg)
	* a baseline for comparison.  serialization delays. shallow buffered vs deep buffered (arista) switches
* WiFi, (DaveT)
* DOCSIS,  (Greg, Jason)
	* expected latency due to DS serialization/framing, US MAC Req-Grant, discontinuous link (pkt aggregation).  Support for buffer control, AQM, LLD.
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
* FIFOs
	* baseline case, most widely deployed in network gear, discuss buffer sizing, relation to congestion control, buffer bloat
* AQMs: CoDel, PIE, DOCSIS-PIE, Cobalt, etc.
	* very brief explanation of what AQM is, mention of algorithms commonly in use in networks today, impact on latency/loss tradeoff
* Flow queuing: fq\_codel, fq\_pie, CAKE
	* very brief explanation of FQ,where is it deployed, expected result

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
	* identification of traffic importance and/or QoS sensitivity, SLAs, prioritization, policing, access control, admission control, end-to-end issues at layer 8+ for Internet traffic
* Explicit Congestion Notification (Greg)
	* what is it? recent data on deployment, segue to L4S
* QB/NQB distinction & Low-Latency, Low-Loss, Scalable throughput architecture (Greg)
	* brief overview & pointers 

# Metrics and methods for characterizing latency performance (5pgs)

## Latency for a path in a live network is variable – a statistical distribution (Greg)
* Descriptive statistics: Average latency, min/mean/max/std
	* in measurements, "latency" almost always equates to average latency
	* average latency is a pretty poor metric for judging QoE 
	* of the other "easy" statistics that commonly get reported (min, max, std), min is reasonably useful, max and std are pretty worthless
* Order statistics (P0,P99,P99.9)
	* P99 packet latency (or possibly P99.9) much more useful to predict QoE
	* P0 (min latency) is helpful too 
* Packet Delay Variation vs. Inter-Packet Delay Variation vs. “Jitter”
	* There are at least 9 different definitions of "jitter" in common use in the industry, each produces a markedly different value (even two orders of magnitude different!) given a sequence of packet latency observations
	* issues with IPDV and out-of-band measurement
* CDFs
	* examples of CCDF on log scale	
* "Responsiveness" RPM (round-trips per minute)
	* discuss pros/cons of this metric, 
	* pros: bigger is better, units are understandable by lay-people
	* cons: implies a sequence of events in series, it's based on average latency

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
* Motion-to-photon latency and sickness
* components of the end-to-end latency chain
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
