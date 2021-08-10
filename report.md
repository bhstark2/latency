\newpage

**Executive Summary**

Placeholder text: When people think of Internet performance it is typically solely in terms of speed. But when considering the end user quality of experience (QoE), latency is usually a key factor. Unfortunately, latency is not well understood in the policy or regulatory spheres or by the average consumer. This paper will explain and explore latency, including idle latency, latency under load, components of latency performance, how latency varies by access technology and/or protocol, and how latency can affect the user QoE for certain types of applications such as video conferencing, remote learning, gaming and more. As well, while the FCC MBA program reports on latency it takes no position on what is good or bad latency - a question this paper will explore. Additionally, this paper will examine metrics for characterizing latency performance, including various summary statistics for latency and latency variation (often referred to as "jitter"). Finally, the paper will look to the current state of the art and the future to explain Active Queue Management (AQM) and other low latency services - and the new types of applications that this may enable in the future.

\pagebreak


# Introduction
Today when people think of end user Internet performance, it is typically solely in terms of speed -- the aggregate capacity of a home's Internet connection. Recent debate in the U.S. about improving performance has as a result centered around the potential benefits of moving to symmetric gigabit connections [https://www.eff.org/deeplinks/2021/07/future-symmetrical-high-speed-internet-speeds] or dramatic increases in upstream speeds [https://arstechnica.com/tech-policy/2021/03/100mbps-uploads-and-downloads-should-be-us-broadband-standard-senators-say/] as a tool to solve performance problems, particularly with interactive applications such as video conferencing that have seen recent mass adoption as a result of the COVID-19 pandemic [https://www.bitag.org/documents/bitag_report.pdf]. This primary focus on speed has been consistent with the Internet industry's approach for more than twenty-five years that simply increasing speed was the best way to improve end user performance, often called the end user’s Quality of Experience (QoE). 

But over the last decade, the industry, including protocol developers [refs], researchers [refs], operating system developers [refs], application providers [refs], Internet Service Providers (ISPs) [refs], and standards development organizations [ref to recent IAB workshop report and IETF TSV WG materials] have slowly begun to recognize that another key factor significantly affecting QoE is “latency”, which is critical to any application involving user interaction from web browsing to video streaming and everything in between. As this is a recent development, it has meant that latency is not well understood in the policy or regulatory spheres or by the average consumer – or even by many in the industry. One of the reasons for this has been that there has historically been a test of latency, but that test has very little to do with end user QoE.

While latency may sound technical or complicated, what it represents is quite simple: latency is simply *delay* – the time between asking for something on the Internet and receiving it. The less delay that a network or application has, the more “responsive” a service will feel to an end user. The more delay (or lag), the worse it will feel, and every user is familiar with user interface cues that reflect this – such as a spinning wheel to communicate waiting or buffering. Having low delay is centrally important to any application involving a user interacting with a device or application, and will become ever more so as new cloud-based applications, augmented reality, virtual reality, and other new application classes emerge. Critically, however, reducing delay (latency hereafter) will *meaningfully improve all existing user applications*.

In addition, it is also important to have a *consistently responsive* service where delay stays consistently low no matter how heavily utilized a user’s Internet connection may be and no matter what mix of applications are being used. Another way of saying this is that delay should not vary by much; the less variability the better. If this is not so, as is often the case on the Internet today, then delay can vary wildly from very good to very bad. To an end user that variability ends up being very confusing – one moment their network and applications appear to perform well and the next moment they do not. They often just accept “that’s how the Internet works” and while there was a bit of a hiccup one moment everything seems okay now, so they ignore this oddly variable performance. Network engineers use the term “jitter” when describing the variability of latency, which we will use and explain later in the report.

The main test of latency today, the results of which are listed in reports such as the FCC’s Measuring Broadband America (MBA) report [see Section D of the 10th FCC MBA report at https://www.fcc.gov/reports-research/reports/measuring-broadband-america/measuring-fixed-broadband-tenth-report], are really what is becoming better understood as a test of “idle latency”. Such an idle latency test is typically performed by sending a single ping packet from a home to a destination server on the Internet and reflects the round trip time (RTT) for the packet to travel to the server and back. However, these tests are typically performed with no other meaningful traffic utilizing the end user’s Internet connection at the exact moment if the idle latency test. 

Idle latency tends to reflect the nothing more that the inherent attributes of different access network technologies, such as minor differences in the way that packets are handled in a fiber to the home (FTTH), hybrid-fiber coax (HFC), Digital Subscriber Line (DSL), Wi-Fi, satellite, or other access network type. While at the extremes in comparing for example FTTH and satellite, the differences in idle latency may be somewhat significant, by and large the gap between different wired connections is insignificant and does not have a significant impact on end user QoE. In contrast, differences in working latency can be significant and that measure is more directly representative of real-world performance. Idle latency can therefore be considered a mildly interesting reflection of access network technology and network topology but when it comes to how end users experience the Internet it is nothing more than a measurement of a connection when it is not being actively used and is therefore essentially meaningless to end user QoE. *To really understand end user QoE, we need to look at working latency rather than idle latency.*

When a longer test of latency is run at the same moment that the Internet connection is being utilized, this is a test of “working latency”. Working latency is therefore a better reflection of the real-world performance of an end user’s Internet connection. The difference between idle latency and working latency can be significant – on the order of hundreds of milliseconds to several seconds. That difference is the difference between a player losing an online game, between a good and bad video conference experience, between slow start of video streaming playback and instant playback, etc. The differences between working latency from one network to another and one user to another at the current time are significant and bear as directly on end user QoE as connection speed. 

A test of working latency typically involves running a speed test (a large file transfer) to heavily utilize a connection while in parallel running a series of ping tests to a destination server. This will show the packet delay when a connection is in use, reflecting real-world performance. To best measure what performance a user might expect, working latency tests will generally represent both the median of these measurements as well as the maximum and so therefore average (mean) and minimums are generally not useful or reflect expected realistic end user QoE. That is because the minimum is likely close to or the same as idle latency and the average is not very useful if latency is highly variable across a wide spectrum of hundreds or thousands of milliseconds. So when we look at working latency, we will focus on the median and maximum measurements.

To summarize, idle latency is mildly interesting but does not significantly influence end user QoE, while in contrast working latency is centrally important to that QoE. As well, it is important to have consistently low working latency. If both can be achieved, a user will perceive a service as being consistently responsive to their application needs and experiences. *The best Internet connection will have both high throughput (high speed), low delay (low latency), and consistent delay (low jitter).*

But what causes working latency to increase? The best way to understand that is to first envision the end-to-end path from a user to a destination server. A user has a device such as a laptop or mobile device, which connects to their Local Area Network (LAN, often connected via Wi-Fi), to some Customer Premises Equipment (CPE) like a home router or cable modem gateway, to an Internet Service Provider (ISP) network and across that network, then across one or more interconnection points between to networks to a destination network, to network devices in a destination data center (e.g., router, load balancer, switch), and eventually to a destination server. Along this end-to-end path, the link with the least capacity in the upstream and downstream direction (which may be different for each direction), which is the most constrained link, is typically referred to the “bottleneck link”. This bottleneck link – and there is always a bottleneck link on any path - requires a buffer of some sort to moderate the high flow of packets coming into a link with the lower flow of packets that the next link in the path can accept. Thus, buffer, or queue, in that bottleneck link will be the source of any latency. 

A good way to envision a buffer is to envision pouring water into a funnel that has a wide top (lots of bandwidth capacity) and a narrow bottom (lower bandwidth capacity). As a large volume of water enters the funnel, at some point the incoming water flow is greater than can be drained at the bottom and the water level begins to rise – so a buffer of water forms and the water line in the funnel begins to rise. When water rises over the top of the funnel and spills out (akin to packet loss) this may be a signal for someone to slow the rate of incoming water flow. Or the person holding the funnel may tell the person pouring water to slow down (akin to congestion notification). Whether as a result of noticing water spilling over the top of the funnel or receiving a signal from the person holding the funnel to slow down, the person then begins to reduce the volume of their pour. As the incoming flow of water moderates, and the volume between input and output equalizes, the water level in the funnel (buffer) can then begin to drain. 

There will always be a constrained link on an Internet path, due to its packet-based design which means in part that it does not create end-to-end circuits with dedicated capacity. As a result, the presence of a bottleneck link and a buffer is not good or bad – it is merely a fact of the design of the Internet. As well, the fact that some packets are lost at this bottleneck is also completely normal and indeed is essential, as this is a key part of the control signal or feedback loop to a sender to moderate how fast they are sending packets. Without trying to send at an increasingly higher rate, a sender would otherwise never be able to discover and fully utilize the maximum capacity of an end-to-end path, nor adjust to take advantage of new capacity as it becomes available. Thus, what’s critical is not that a queue exists or that packet loss will occur - it is how that buffer or queue performs and how quickly it can react to and communicate changing conditions.

Network engineers refer to how a buffer performs as “queue behavior”. The predominant and most basic way to manage the behavior of a queue today is First In First Out (FIFO) which is easy to understand [ref to FIFO queue management]. But newer forms of queue management, such as various types of so-called Active Queue Management (AQM) [ref to overview of several AQMs from fq_codel to PIE], have started to emerge in recent years that can dramatically improve the responsiveness of these network queues and therefore dramatically lower working latency.  

This paper will further explore all of these topics, from a deeper dive into latency measurement to the newest forms of AQM and where that AQM is best deployed. 


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
