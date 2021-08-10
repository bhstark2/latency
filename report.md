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


# Definition of Latency (David to review this text)

A good starting point is to first understand idle latency, which reflects the underlying and inherent latency of an end-to-end path, because working latency simply builds on top of that. The latency between a laptop and the next hop of a packet on the LAN will typically be quite short. As you add successive network hops from the laptop to the home gateway, then the ISP network, and all the way to the destination server, the latency will increase as each new link in the chain is added. But the number of links in and of itself does not necessarily mean greater latency per se.

This is because some links can be quite short, such as a fiber link between a data center switch and a destination server, while other links can be long, such as an east-west fiber link across the U.S. And physical media such as fiber is limited by the physics of the speed of light. This means that, generally speaking, latency increased as distance increases: the time to send a packet across town will be less that the time to send a packet across the country. 

Latency also varies by different types of physical media or type of network (e.g., type of ISP access network technology). For example, looking at the physical media, a wired gigabit fiber connection will typically latency that a copper-based Fast Ethernet connection. In addition, the latency properties of ISP access network technologies will also cause latency to vary – such as the difference between FTTH, DSL, HFC, Wi-Fi, 5G, geostationary satellite, low Earth orbit (LEO) satellite, etc. As a result, idle latency tests of an end-to-end path will simply reflect (1) distance and (2) underlying network technologies. This is the baseline latency that is the starting point for understanding real world end user performance.

However, latency also varies, and significantly so, based on underlying network conditions. That means that latency may increase as traffic volume increases or as the capacity of a connection fills up. It can also mean that latency varies as a result of a mix of different kinds of traffic on the network (e.g., bulk downloads and online game play). When adding in real traffic of any type and volume, we can then see how the network reacts under real-world conditions and understand the so-called working latency of the path.

Finally, since latency is a measure of delay, it is a measure of time. Network latency is typically described in milliseconds of time, though it can often grow to several seconds of time. While idle latency seeks to measure the baseline latency performance of a given end-to-end path, working latency measures the latency performance under real-world conditions when a user’s connection is utilized to a normal extent. 

(missing: Impact that data rate has on latency?)
![image](https://user-images.githubusercontent.com/8984861/128738550-b73732e2-a856-4a14-ae25-0ed223f72884.png)



# Sources/Contributors to Latency (8pgs)

## Link technologies in place along the path (3pgs)

*Should this section be "Physical-layer Technologies in Place along the Path"? Would it be useful to remind readers of the layered stack: physical, link, IP, application and step up the stack? I re-ordered the bullets in this section a little to group all the LAN technologies towards the beginning, then access, but with Ethernet staying first. They were mostly already ordered that way.*

Physical-layer technologies in the path can contribute to latency due to speed at which signals are transmitted on the physical medium, the distance the signals travel (length of the physical medium), the time it takes for equipment to encode and decode the physical and link-layer technologies, and whether the physical-layer technology uses time-based interleaving to better detect and correct lost bits on noisy loops. The more shielded a physical medium is against noise, the less the physical-layer encoding needs to account for the impacts of potential noise.

### Ethernet, (Greg)

### WiFi, (DaveT)

### Powerline Carrier (PLC) (Barbara)

Several physical-layer technologies have been defined for use on powerlines. While powerline has been explored for broadband access, it is not widely used as an acces technology in the United States. The dominant PLC technologies for use in a LAN are HomePlug and G.hn (standardized by ITU-T). Latency due to speed of transmission and distance are negligible on the short LAN loops where PLC is used.

HomePlug products are still being deployed in LANs, but the HomePlug Alliance (which provided advocacy and certification) is no longer active. There is no interoperability among the various HomePlug silicon solutions and no detailed specification of the technology. Measurable latency has been noted in some HomePlug networks, which suggests some HomePlug  products may make use of time-based interleaving.

G.hn does not use time-based interleaving. In addition to operating on LAN powerlines, G.hn is also often used on coax and twisted pair and is being deployed in some Multi Dwelling Unit (MDU) environments to provide broadband access to individual units.

### DOCSIS,  (Greg, Jason)
 
### Digital Subscriber Line (DSL) and G.fast

ADSL, ADSL2+, VDSL, VDSL2, and G.fast are "last mile" broadband access technology standards defined by ITU-T. All of these are primarily defined to run over twisted-pair copper wires, although G.fast can also run over coax (which is useful in some multi-dwelling unit deployments). The older Asymmetric Digital Subscriber Line (ADSL) technology was generally used on loop lengths of 1 mile or less. The newer ADSL2+, Very high-speed DSL (VDSL) and VDSL2 technologies are generally used on shorter loops in a fiber to the node (FTTN) configuration (with copper to a node and fiber from the node to the central office). Since copper is a very efficient transmission medium, the time for a signal to travel these distances is very small and does not contribute significantly to latency. Encoding and decoding DSL signals does add some small latency. But this delay is also very small.

Some ADSL2+ and VDSL deployments used a time-based interleaving technique to be more resilient against noise on the line. Noise can result in lost bits of data. With interleaving, it is often possible for the ADSL2+ or VDSL receiver to recover these lost bits. If lost bits are not recovered, the loss can result in either missing information (e.g., clipped sound in an audio transmission) or cause the data to be retransmitted (which causes delay). But time-based interleaving adds delay to accomplish this resiliency. Common interleaving delays range from 2 - 10 ms, when it is enabled.

G.fast is another copper-based technology (over twisted pair or coax) that can be used on very short loops (up to around 500 ft). Interleaving is not used with G.fast and the loop length and encoding mechanisms add very small latency.

When interleaving is not used, latency of these technologies tends to be measured in nanoseconds. 

*I need to see if I can bet more precise latency calculations/numbers and references for this section.*

### PON, (Barbara)

Brief discussion of speed of light in glass. 

### LTE/5G, (Barbara)

### Satellite (DaveT)
	* media access, scheduling, etc.
	
### Core network links (Shamim)
A. Topology: middle mile, edge, backbone and submarine links
B. Speed of light 
C. Routing 
D. Geolocation data anomalies and edge miss mappings
E. eDNS client subnet
F. Direct peering vs. Transit 

## Buffering delays (3pgs)
### Impact that senders & network protocols have on path latency (Koen)
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

## VPNs and Proxied Paths (1pg) (Barbara, Sam)
* iCloud/Safari Private Relay, etc.


# Current and Future Technologies to improve latency performance (3pgs) (Shamim)
* Migration to the network edge (CDNs, MEC) (Shamim)
A. Edge cache vs. Edge compute
B. Video vs. Real time traffic handling
C. Near real time inference and actions
D. Down Link vs. Uplink bandwidth requirement 
E. Multipath from user via both Wired broadband and 5G to compute/content Edge
E. Geographic challenges and need for carrier neutral MEC
F. On premise edge compute for campus and arena 
G. On device compute evolution that nullifies or complements the MEC requirement 

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

## Measuring latency (Sam)
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

## Multiplayer online games (Alex)

Multiplayer online games are particularly sensitive to latency. This is especially true for fast-paced games, such as first person shooters (FPS) or sports games. Even in the early days of such games in the late 1990s, gamers were acutely aware of the impact of latency on their experience.

For the avoidance of doubt, we are concerned only with gameplay here, which is highly latency sensitive. Modern games will also download game updates, content for future parts of the game (videos, animations, maps, etc), which are bulk downloads and generally not immediately visible to the user, so are not latency sensitive.

Excessive latency, commonly referred to as ‘lag’ or ‘ping’ by gamers, can cause a variety of issues during gameplay. One of the most common is 'rubber banding', which is the appearance that a character has moved to one location and then immediately snaps back to another (we will explore this further below). Another related one is the perception that some players can shoot around corners, i.e. your player is shot even though, from your point of view, the shooter was around a corner at the time. A third is the ‘peekers advantage’, whereby someone peeking around a corner can see their opponent before they see them.

There are many causes of latency in games. Of course, the network is a key one and receives a lot of focus within games (many games have the ability to report your 'ping' on the screen during gameplay). There are many others though. The screen in use can play a factor, which is why many modern TVs now offer a 'game mode' and why monitors are being advertised with low response times and high refresh rates. Input devices, such as game controllers or keyboards/mice can have an impact too. For the purposes of this section we will focus largely on the network though.

Modern FPS games operate using a client-server model. The server maintains the authoritative state of the world for all players. The server updates its internal state at a fixed interval, based upon the inputs received from all players. This update process is known as the games 'tick rate' and is typically 64Hz (once every 16.6ms) for a FPS, but the very latest ones are moving to 128Hz [2]. Once the server has updated its internal state, it transmits updates to all of the players. This update rate is slower than the frame rate, which means that interpolation must be used in order for smooth animation to occur.

All of this communication is typically over UDP, but with their own minimal reliability mechanism layered on top. For example, this may take the form of including a sequence and acknowledgement number in each packet, but instead of retransmitting lost segments, the sender will just transmit all unacknowledged data in subsequent packets.

Of course, different players will have different latency to the server, and these can change during the course of gameplay too. Without any mitigations for this, a player with a high latency could shoot another and think they scored a direct hit, but the other player with a low latency may have already moved out of the way from the server's perspective, so a hit was not recorded. This is unfair on players with higher latencies, so as a result game developers have developed lag compensation techniques to counteract this.

A common method of lag compensation involves the server keeping a short history of snapshots of its state of the world. Then, when a player shoots at another player, the shooting player transmits its version of the state of the world back to the server along with its state update ("I fired a shot"). The server can now, if necessary, rewind its global state back to the shooting player's version and 'see' exactly what the shooting player saw, including whether the shot was on target from the shooter's perspective. If so, then an adjustment is made to the global state and an update is transmitted to all players to say that the target was in fact shot after all.

This process has some subtleties to it. Firstly, it introduces the problem with the appearance that some players can shoot around corners. Game developers tend to take the opinion that this is a better outcome than lagged players being horribly disadvantaged. Secondly, a history of state snapshots needs to be maintained so that this rewinding can occur. This requires additional memory and processing, and care needs to be taken to not set the limits too high, otherwise one highly lagged player can lead to a poor performance for every player. Lastly, the client side will apply its own movement updates entirely locally and not wait for feedback from the server; this ensures that if a player presses the forward key then the movement is registered immediately. Of course, this is only applied for some actions by the client (like movement) and not others (like shooting an opponent).

Lastly, some games try to predict the likely future movement of users. This, when coupled with bufferbloat, can lead to the ‘peekers advantage’ effect. This arises because the peeking player’s client side has predicted they will look around a corner, shown them the opposing player, and meanwhile the movement updates are stuck in an outbound queue, meaning that the opposing player will not see their presence for some time to come. Riot games reported this as delivering a 141ms advantage in some cases, and have reduced deployed workarounds to counteract this [3].

Game developers are acutely aware of latency and how it can occur at all layers in the stack. Slides from the Games Developer Conference in 2016 show Activision, a large games publisher, using high speed cameras to measure motion-to-phone latency in order to debug a lag issue in a new game. See [1].

[1] https://ubm-twvideo01.s3.amazonaws.com/o1/vault/gdc2016/Presentations/Goyette_Benjamin_Fighting_Latency_COD.pdf
[2] https://win.gg/news/4379/explaining-tick-rates-in-fps-games-difference-between-64-and-128-tick
[3] https://technology.riotgames.com/news/peeking-valorants-netcode


## Cloud gaming (Alex)

The past few years has seen the emergence of 'cloud gaming' platforms, such as Google's Stadia or nVidia's GeForce Now. These move the graphics intensive work to the server side and then effectively transmit the video of the gameplay back to the client side. This allows the game to run on far lower specification devices, without the need for powerful GPUs client side. The developers of these platforms say that increases in internet access bandwidth and decreases in latency make this approach possible.

This of course means that cloud gaming platforms are now more bandwidth intensive than traditional games, as they have to transmit high resolution video at a high frame rate to the client side. GeForce Now, for example, states that it "requires at least 15Mbps for 720p at 60fps and 25 Mbps for 1080p at 60fps".

There is also anecdotal evidence that suggests that high or erratic latency may also be used by cloud gaming platforms as a signal to reduce the resolution of the game. Little published material is available from the cloud gaming platforms to explain their approach in technical detail.

All of the issues discussed in the above multiplayer online gaming section are true for cloud gaming too. Plus, there are some interesting additional issues to consider. In the previous section we discussed that the client can apply its own local movement updates in order to show immediate feedback to the local player (even if the remote players will not perceive it for some milliseconds yet). The ability to do this disappears with cloud gaming. All of the inputs must be sent to the server side, processed, and then transmitted back to the client for display. This can lead to a player pressing the shoot button, and then not seeing that take effect on screen until a short time later. This leads to a significantly higher 'controller-to-photon' delay than with a local-only game.

An example of this phenomenom can be seen in a PC Gamer article at https://www.pcgamer.com/uk/heres-how-stadias-input-lag-compares-to-native-pc-gaming/




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
