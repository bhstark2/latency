\newpage

**Executive Summary**

Placeholder text: When people think of Internet performance it is typically solely in terms of speed. But when considering the end-user quality of experience (QoE), latency is usually a key factor. Unfortunately, latency is not well understood in the policy or regulatory spheres or by the average consumer. This paper will explain and explore latency, including idle latency, latency under load, components of latency performance, how latency varies by access technology and/or protocol, and how latency can affect the user QoE for certain types of applications such as video conferencing, remote learning, gaming and more. As well, while the FCC MBA program reports on latency it takes no position on what is good or bad latency — a question this paper will explore. Additionally, this paper will examine metrics for characterizing latency performance, including various summary statistics for latency and latency variation (often referred to as "jitter"). Finally, the paper will look to the current state of the art and the future to explain Active Queue Management (AQM) and other low latency services — and the new types of applications that this may enable in the future.

\pagebreak


# Introduction
Today when people think of end user Internet performance, it is typically solely in terms of speed -- the aggregate capacity of a home's Internet connection. Recent debate in the U.S. about improving performance has as a result centered around the potential benefits of moving to symmetric gigabit connections [https://www.eff.org/deeplinks/2021/07/future-symmetrical-high-speed-internet-speeds] or dramatic increases in upstream speeds [https://arstechnica.com/tech-policy/2021/03/100mbps-uploads-and-downloads-should-be-us-broadband-standard-senators-say/] as a tool to solve performance problems, particularly with interactive applications such as video conferencing that have seen recent mass adoption as a result of the COVID-19 pandemic [https://www.bitag.org/documents/bitag_report.pdf]. This primary focus on speed has been consistent with the Internet industry's approach for more than twenty-five years that simply increasing speed was the best way to improve end-user performance, often called the end user’s Quality of Experience (QoE).

But over the last decade, the industry, including protocol developers [refs], researchers [refs], operating system developers [refs], application providers [refs], Internet Service Providers (ISPs) [refs], and standards development organizations [ref to recent IAB workshop report and IETF TSV WG materials] have slowly begun to recognize that another key factor significantly affecting QoE is “latency”, which is critical to any application involving user interaction, from web browsing to video streaming and everything in between. As this is a recent development, it has meant that latency is not well understood in the policy or regulatory spheres or by the average consumer — or even by many in the industry. One of the reasons for this has been that there has historically been a test of idle (best-case) latency, but that test has very little to do with end-user QoE.

## Definition of Latency

While latency may sound technical or complicated, what it represents is quite simple: latency is simply *delay* — the time between asking for something on the Internet and receiving it. The less delay that a network or application has, the more “responsive” a service will feel to an end user. The more delay (or lag), the worse it will feel, and every user is familiar with user interface cues that reflect this — such as a spinning wheel to communicate waiting or buffering. Having low delay is centrally important to any application involving a user interacting with a device or application, and will become ever more so as new cloud-based applications, augmented reality, virtual reality, and other new application classes emerge. Critically, however, reducing delay (latency hereafter) will *meaningfully improve all existing user applications*.

In addition, it is also important to have a *consistently responsive* service where delay stays consistently low no matter how heavily utilized a user’s Internet connection may be and no matter what mix of applications are being used. Another way of saying this is that delay should not vary by much; the less variability the better. If this is not so, as is often the case on the Internet today, then delay can vary wildly from very good to very bad. To an end user that variability ends up being very confusing — one moment their network and applications appear to perform well and the next moment they do not. They often just accept “that’s how the Internet works” and while there was a bit of a hiccup one moment everything seems okay now, so they ignore this oddly variable performance. Network engineers use the term “jitter” when describing the variability of latency, which we will use and explain later in the report.

The main test of latency today, the results of which are listed in reports such as the FCC’s Measuring Broadband America (MBA) report [see Section D of the 10th FCC MBA report at https://www.fcc.gov/reports-research/reports/measuring-broadband-america/measuring-fixed-broadband-tenth-report], are really what is becoming better understood as a test of *idle latency*. Such an idle latency test is typically performed by sending a single ping[^1] packet from a home to a destination server on the Internet and reflects the round trip time (RTT) for the packet to travel to the server and back. However, these tests are typically performed with no other meaningful traffic utilizing the end user’s Internet connection at the exact moment if the idle latency test. 

[^1]: ping is discussed further in section XX 

Idle latency tends to reflect the nothing more that the inherent attributes of different access network technologies, such as minor differences in the way that packets are handled in a fiber to the home (FTTH), hybrid fiber-coax (HFC), Digital Subscriber Line (DSL), Wi-Fi, satellite, or other access network type. While at the extremes in comparing for example FTTH and satellite, the differences in idle latency may be somewhat significant, by and large the gap between different wired connections is insignificant and does not have a significant impact on end-user QoE. In contrast, differences in *working latency* can be significant and that measure is more directly representative of real-world performance. Idle latency can therefore be considered a mildly interesting reflection of access network technology and network topology but when it comes to how end users experience the Internet it is nothing more than a measurement of a connection when it is not being actively used and is therefore essentially meaningless to end-user QoE. *To really understand end-user QoE, we need to look at working latency rather than idle latency.*

When a longer test of latency is run at the same moment that the Internet connection is being utilized, this is a test of working latency. Working latency is therefore a better reflection of the real-world performance of an end user’s Internet connection. The difference between idle latency and working latency can be significant — on the order of hundreds of milliseconds to several seconds. That difference is the difference between a player losing an online game, between a good and bad video conference experience, between slow start of video streaming playback and instant playback, etc. The differences between working latency from one network to another and one user to another at the current time are significant and bear as directly on end-user QoE as connection speed.

A test of working latency typically involves running a speed test (a large file transfer) to heavily utilize a connection while in parallel running a series of ping tests to a destination server. This will show the packet delay when a connection is in use, reflecting real-world performance. To best measure what performance a user might expect, working latency tests will generally represent both the median[^2] of these measurements as well as the maximum and so therefore average (mean) and minimums are generally not useful or reflect expected realistic end-user QoE. That is because the minimum is likely close to or the same as idle latency and the average is not very useful if latency is highly variable across a wide spectrum of hundreds or thousands of milliseconds. So when we look at working latency, we will focus on the median and maximum measurements.

[^2]: footnote to explain median and mean

To summarize, idle latency is mildly interesting but does not significantly influence end-user QoE, while in contrast working latency is centrally important to that QoE. As well, it is important to have consistently low working latency. If both can be achieved, a user will perceive a service as being consistently responsive to their application needs and experiences. *The best Internet connection will have high throughput (high speed), low delay (low latency), and consistent delay (low jitter).*

## Causes of Latency

But what causes poor working latency? The best way to understand that is to first envision the end-to-end path from a user to a destination server. `can this be a figure instead:` __A user has a device such as a laptop or mobile device, which connects to their Local Area Network (LAN, often connected via Wi-Fi), to some Customer Premises Equipment (CPE) like a home router or cable modem gateway, to an Internet Service Provider (ISP) network and across that network, then across one or more interconnection points between to networks to a destination network, to network devices in a destination data center (e.g., router, load balancer, switch), and eventually to a destination server.__ Along this end-to-end path, the link with the least capacity in the upstream and downstream direction (which may be different for each direction) is the most constrained link, and is typically referred to the “bottleneck link”. This bottleneck link — and there is always a bottleneck link on any path — requires a buffer of some sort to moderate the high flow of packets coming into a link with the lower flow of packets that the next link in the path can accept. Thus, the buffer, or queue, in that bottleneck link can contribute significantly to latency.

A good way to envision a buffer is to envision pouring water into a funnel that has a wide top (lots of bandwidth capacity) and a narrow bottom (lower bandwidth capacity). As a large volume of water enters the funnel, at some point the incoming water flow is greater than can be drained at the bottom and the water level begins to rise — so a buffer of water forms and the water line in the funnel begins to rise. When water rises over the top of the funnel and spills out (akin to packet loss), this may be a signal for someone to slow the rate of incoming water flow. Or the person holding the funnel may tell the person pouring water to slow down (akin to congestion notification). Whether as a result of noticing water spilling over the top of the funnel or receiving a signal from the person holding the funnel to slow down, the person then begins to reduce the volume of their pour. As the incoming flow of water moderates, and the volume between input and output equalizes, the water level in the funnel (buffer) can then begin to drain.

There will always be a constrained link on an Internet path, due to its packet-based design which means in part that it does not create end-to-end circuits with dedicated capacity. As a result, the presence of a bottleneck link and a buffer is not good or bad — it is merely a fact of the design of the Internet. As well, the fact that some packets are lost at this bottleneck is also completely normal and indeed is essential, as this is a key part of the control signal or feedback loop to a sender to moderate how fast they are sending packets. Without trying to send at an increasingly higher rate, a sender would otherwise never be able to discover and fully utilize the maximum capacity of an end-to-end path, nor adjust to take advantage of new capacity as it becomes available. Thus, what’s critical is not that a queue exists or that packet loss will occur — it is how that buffer or queue performs and how quickly it can react to and communicate changing conditions.

Network engineers refer to how a buffer performs as “queue behavior”. The predominant and most basic way to manage the behavior of a queue today is First-In First-Out (FIFO), which is easy to understand [ref to FIFO queue management]. But newer forms of queue management, such as various types of Active Queue Management (AQM) (discussed further in Section XX), have started to emerge in recent years that can dramatically improve the responsiveness of these network queues and therefore dramatically lower working latency.  

This paper will further explore all of these topics, from a deeper dive into latency measurement to the newest forms of AQM and where that AQM is best deployed. 

# Definitions 
## Speed or Throughput (Stuart)

What many end users think of as the “speed” of their Internet connection
is what Internet engineers refer to using terms like
“bandwidth”, “capacity”, and “throughput”.
When we describe a link capacity as
1 kilobit per second, or
1 megabit per second, or
1 gigabit per second,
that refers to the *amount* of data passing through a given point per second,
not how *quickly* that data travels to its final destination.
These are a measure of capacity, not speed.

Notice that the term is “band-width”, not “band-speed”.

Imagine filling a bucket with a hose pipe.
If the bucket is taking too long to fill, we can get a fatter hose pipe.
The bucket now fills quicker,
not because the water is moving through the pipe at a faster speed,
but because there is more water in the pipe.
Because the pipe is wider, even if the individual water molecules are
traveling down the pipe at the same speed, there are more of them,
so the bucket fills faster.

To use another analogy,
increasing bandwidth is like adding more lanes to a highway —
it makes the highway wider, so it can carry more cars,
but it doesn’t change the speed limit.
Counting cars-per-minute passing a certain point on a highway
tells you about the capacity (or width) of the highway,
but it tells you nothing about the speed of the individual cars.
Analogies are often imperfect, and this analogy,
comparing network traffic to road traffic, also has to be interpreted carefully.
Unlike cars on a road, which have human drivers who slow down in heavy traffic,
photons traveling through glass fiber
(or electrical signals traveling through wire)
move at a constant speed.
Photons do not slow down because there are other photons ahead of them in the fiber.

So, if photons and electrical signals travel at a constant speed and never
slow down, how do computer networks experience congestion that slows down
the responsiveness of the network and degrades user experience?

Delays in computer networks do not occur in the cables;
they occur in the switching equipment that connects the cables.
When a data packet arrives in a piece of switching equipment,
and the cable on which the packet is supposed to depart is already busy,
the data packet has to wait its turn.
If there are many other packets similarly waiting,
the data packet may have to wait a significant amount of time.

Another term commonly used by Internet engineers is “latency”.
The word “latent” means “hidden”,
and latency is the “hidden” component of delay.

If we send 50 kilobytes of data using an old dial-up modem,
we expect that to take a few seconds.
If we send 500 kilobytes of data,
we would expect that to take ten times longer.
And if we send 500 megabytes,
we would expect that to take 1000 times longer than 500 kilobytes.

If we plot this data on a chart, we’d expect to see a straight line,
where the more data is sent the longer it takes.
But if we take that same straight line and extrapolate it back in
the other direction, towards a hypothetical “zero sized” message,
we find that the point where our straight line crosses the time
axis is not zero.
Even for a hypothetical zero-sized message,
the time it takes to deliver it is non-zero.
The time to deliver a message is made up of an overt component,
related to the size of the message
divided by the transmission rate of the medium,
and a hidden component, which doesn’t vary with message size.
As the transmission rates of home Internet connections
have grown from
kilobits per second to
megabits per second and even
gigabits per second
the overt component of delay — the size-related component —
has become negligible for all but the largest data transfers.
But the hidden component of delay — the size-invariant latency —
has remained mostly unchanged since the Internet’s birth in the 1980s.

This is how we have arrived at an Internet today where
the end-user experience is now determined almost entirely by the latency
— the hidden size-invariant component of delay —
and hardly at all by the commonly measured
bandwidth, capacity, or throughput of the connection.

If you want to drive from New York to San Francisco,
a moving truck will not get you to San Francisco
faster than a four-door family saloon car.
Certainly, if you are moving house and have a lot of possessions to
transport then the moving truck has a higher cargo *capacity* than
the four-door family saloon car, but it won’t get you there *faster*.

## Latency

Network latency is a core characteristic of broadband service that is receiving increasing attention as it has a significant impact on how well applications work over the Internet. Latency has been an accepted performance metric of interest since the inception of modern data communications based on digital technology. Stated formally, network latency represents the time that it takes for data packets to travel from one network host to another network host. Since data packets cannot instantaneously be sent from the source to its destination, the network latency metric provides a measure of the total delay experienced by the packet as it is transmitted through many different network nodes in order to arrive at its intended destination. For residential ISPs, this metric can often be further focused to reflect downstream latency and upstream latency to measure network delays encountered in sending data in the downstream direction through the network to the end user, or in the upstream direction from the end user into the core network, respectively. 

The network latency encountered in nominal or light usage conditions is known as the idle latency of the network. The latency between a laptop and the next hop of a packet on the LAN will typically be quite short. As you add successive network hops from the laptop to the home gateway, then the ISP network, and all the way to the destination server, the latency will increase as each new link in the chain is added. But, because each link may introduce a different amount of latency, a large number of links in and of itself doesn't necessarily mean high latency.

This is because some links can be quite short, such as a fiber link between a switch and a server in a data center, while other links can be long, such as an east-west fiber link across the U.S. And physical media such as fiber is limited by the physics of the speed of light. This means that, generally speaking, latency increases as distance increases: the time to send a packet across town will be less than the time to send a packet across the country. 

Latency also varies by different types of physical media or type of network (e.g., type of ISP access network technology). For example, looking at the physical media, a gigabit fiber connection will typically have lower latency than a copper-based 10Mb/s Ethernet connection. In addition, the latency properties of ISP access network technologies will also cause latency to vary — this subject is discussed futher in the next section.  As a result, idle latency tests of an end-to-end path will simply reflect (1) distance and (2) underlying network technologies. This is the baseline latency that is the starting point for understanding real world end-user performance.

However, latency also varies, and significantly so, based on underlying network conditions. That means that latency may increase as traffic volume increases or as the capacity of a connection fills up. It can also mean that latency varies as a result of a mix of different kinds of traffic on the network (e.g., bulk downloads and online game play). When adding in real traffic of any type and volume, we can then see how the network reacts under real-world conditions and understand the so-called working latency of the path. Thus, the problem with idle latency measurements is that they fail to reliably measure network performance when working traffic loads are present on the network, which can result in misleading characterizations of the network delays present on the network when it is operating under normal load conditions. Latency measurements taken when significant network traffic is present provides a more realistic measure of network delays is known as network latency under load (LUL). 

## Latency and Speed

Latency and speed are two different characteristics of a path between a sender and a receiver, and they are largely orthogonal, but not completely so - they do influence one another.  As we'll describe later in this paper, some existing network protocols are unable to transfer data at high speed when there is significant latency present.  So, high latency can reduce the apparent speed from the user's perspective.  Additionally, increasing the speed of the path can reduce latency in some cases.  One case where this can happen is with latency degradation due to *load* on the path.  For example, a single video stream at 5 Mbps might cause queuing delays 50% of the time on a 10 Mbps connection, but only 5% of the time on a 100 Mbps connection.  To be clear, this is a case of reducing the frequency with which a latency degradation occurs, not the severity of it. This phenomenon, queuing delay, is important and is discussed in some detail in this report.       

# Sources/Contributors to Latency (8pgs)

## Link technologies in place along the path (3pgs)

*Should this section be "Physical-layer Technologies in Place along the Path"? Would it be useful to remind readers of the layered stack: physical, link, IP, application and step up the stack? I re-ordered the bullets in this section a little to group all the LAN technologies towards the beginning, then access, but with Ethernet staying first. They were mostly already ordered that way.*

Physical-layer technologies (meaning... ) in the path can contribute to latency due to the distance the signals travel (length of the physical medium) relative to the speed at which signals are transmitted on the physical medium (propagation delay), the time it takes for equipment to encode and decode the physical and link-layer technologies, and whether the physical-layer technology uses time-based interleaving to better detect and correct lost bits on noisy loops. The more shielded a physical medium is against noise, the less the physical-layer encoding needs to account for the impacts of potential noise.

Propagation delay is determined by the characteristics of the physical medium. The commonly used media of air, fiber and copper have different characteristics, and copper media varies according to thickness (gauge) and shielding used around the copper. The propagation delay of various commonly used media are given in the table below:

Medium  | Round-Trip Delay per Mile
-------|--------------------------
Air (i.e. wireless) | 10.8 µs
Coaxial Cable | 12.5 µs
Optical Fiber | 16.0 µs
Unshielded Twisted Pair | 18.2 µs 

[@fiberlatency] [need additional references]

The majority of networks use fiber optic links for distances longer than a few miles, with microwave links being the second most common [reference?].  For long distance links, the propagation delay difference between these two can be significant (approx. 1.6 ms for fiber vs 1.1 ms for microwave, round-trip for a 100 mile link). 

Access networks generally only use their distinctive medium (air, coaxial cable or unshielded twisted pair) for the last few hundred feet, making the propagation delay differences between these technologies immaterial. 

The more significant differences between access and home network technologies come from other factors such as physical-layer interleaving, FEC encoding and framing, media access delays, and buffering delays. 


### Ethernet, (Greg)

Ethernet links, whether the familiar 1 Gbps LAN cables used to connect devices in home networks and offices, or the 10G, 100G, 200G, 400G optical fiber versions used in datacenters and to connect sites over long distances, form a baseline against which most other network link technologies can be compared.  Historically, the Ethernet standard (IEEE 802) set the maximum size of a packet to be 1500 bytes (to which it adds 18+ bytes of framing).  This *Maximum Transmission Unit* size has been adopted by many other link technologies as well, and thus has become the de facto MTU for the internet.

The latency introduced by an Ethernet link has two components that can be directly calculated from the frame size and the characteristics of the link (speed, distance and medium). For example, a 1518 byte frame (12144 bits) sent via a 1 Gbps interface over a 100-foot copper twisted pair (e.g. Cat6) cable will experience 12.1 microseconds (12144 bits / 1e9 bps) of *serialization delay* (the amount of time it takes to transmit all of the bits of the frame), plus 0.17 microseconds (100 ft / (0.59 * 1000 ft/microsecond)) of *propagation delay* (the time it takes for each bit to make it from the transmitter to the receiver), for a total latency of about 12.3 microseconds.

In addition, an Ethernet *network* (i.e. multiple Ethernet links connected via switches) introduces delays at each switch. The delay added by each switch also has two components: *switching delay* and *buffering delay*.  The switching delay can be substantially less than a microsecond in "cut-through" switches, or can add an amount equal to the serialization delay in "store-and-forward" switches.  The buffering delay is variable, depending on instantaneous traffic load, and can range from 0 to the maximum supported by the switch.  Many switches used in datacenters are "shallow-buffered" such that the maximum buffering delay is on the order of 10s of microseconds, though deep-buffered switches exist as well, supporting maximum buffering delays in the 10s of milliseconds.           

### Wi-Fi

While appearing to the network as if it was an ethernet based device, Wi-Fi is very different in that it not "switched" as is modern day ethernet. Rather, it is more like
early versions of ethernet where only a single transmitter at a time is allowed, with more sophisiticated arbitrage of the basic transmission opportunity (TXOP) than original ethernet.

Potential transmission delays in WiFi can have a range measured in seconds, while competing with a other devices and access points on the same channel, coping with transmission errors
and subsequent retries, and transmission rates can vary also from below 1Mbit to 1Gbit as a function of these problems and of (especially) the distance to the reciever.

Newer standards for Wi-Fi attempt to improve multiplexing behaviors while remaining compatible with older Wi-Fi standards, but co-existing on the same spectrum is difficult,
and standardization more and different spectrum are aiding improvements to Wi-Fi behaviors.

### Powerline Carrier (PLC) (Barbara)

Several physical-layer technologies have been defined for use on powerlines. While powerline has been explored for broadband access, it is not widely used as an acces technology in the United States. The dominant PLC technologies for use in a LAN are HomePlug and G.hn (standardized by ITU-T). Latency due to speed of transmission and distance are negligible on the short LAN loops where PLC is used.

HomePlug products are still being deployed in LANs, but the HomePlug Alliance (which provided advocacy and certification) is no longer active. There is no interoperability among the various HomePlug silicon solutions and no detailed specification of the technology. Measurable latency has been noted in some HomePlug networks, which suggests some HomePlug  products may make use of time-based interleaving.

G.hn does not use time-based interleaving. In addition to operating on LAN powerlines, G.hn is also often used on coax and twisted pair and is being deployed in some Multi Dwelling Unit (MDU) environments to provide broadband access to individual units.

### DOCSIS
	
The Data Over Cable Standard Interface Specifications (DOCSIS) are standards for hybrid fiber-coaxial (HFC) networks (ADD FN: https://www.cablelabs.com/specifications). Most DOCSIS networks today are comprised of primarily DOCSIS 3.0 and 3.1 cable modems and DOCSIS 3.1 Cable Modem Termination Systems (CMTS).  Most HFC networks use coaxial copper cable for the last few hundred feet, and fiber optic cable for the remainder of the distance between the CMTS and cable modems, followed by fiber optic cable from the CMTS to the Internet.

A DOCSIS link is a shared medium.  In the upstream direction, multiple cable modems request access to a particular transmission channel, and access is scheduled by the CMTS.  In the downstream direction, all transmissions are scheduled and made by the CMTS.  As described in Section 1 of [@LLD] there are five sources of latency in DOCSIS 3.1 networks (and similarly DOCSIS 3.0).  These are:  

Delay Source | Range
-------------|------
switching/forwarding | < 0.04 ms
propagation | 0.02 - 0.6 ms
serialization/encoding | 0.4 - 3.5 ms
media acquisition | 2 - 8 ms
queuing | 0 - 200 ms

DOCSIS 3.1 equipment has multiple features to manage latency (some of which are available in DOCSIS 3.0 equipment as well), including Active Queue Management (AQM), and a new feature called *Low Latency DOCSIS* which includes support for the *Low-Latency Low-Loss Scalable Throughput* (L4S) architecture and isolation of Non-Queue-Building (NQB) traffic. AQM, L4S and NQB are discussed later in this document.  

The expected latency performance of these latency management features (as given in Table 1 of [@LLD] in order-of-magnitude numbers) is: 

Feature | When Idle | Under Load (Working Conditions) | 99th Percentile  
--------|-----------|------------|----------------  
Buffer Control | ~10 ms | ~100 ms | ~100 ms
Active Queue Management | ~10 ms | ~10 ms | ~100 ms
Low Latency DOCSIS 3.1 | ~1 ms | ~1 ms | ~1 ms

 
### Digital Subscriber Line (DSL) and G.fast

ADSL, ADSL2+, VDSL, VDSL2, and G.fast are "last mile" broadband access technology standards defined by ITU-T. All of these are primarily defined to run over twisted-pair copper wires, although G.fast can also run over coax (which is useful in some multi-dwelling unit deployments). The older Asymmetric Digital Subscriber Line (ADSL) technology was generally used on loop lengths of 1 mile or less. The newer ADSL2+, Very high-speed DSL (VDSL) and VDSL2 technologies are generally used on shorter loops in a fiber to the node (FTTN) configuration (with copper to a node and fiber from the node to the central office). Since copper is a very efficient transmission medium, the time for a signal to travel these distances is very small and does not contribute significantly to latency. Encoding and decoding DSL signals does add some small latency. But this delay is also very small.

Some ADSL2+ and VDSL deployments used a time-based interleaving technique to be more resilient against noise on the line. Noise can result in lost bits of data. With interleaving, it is often possible for the ADSL2+ or VDSL receiver to recover these lost bits. If lost bits are not recovered, the loss can result in either missing information (e.g., clipped sound in an audio transmission) or cause the data to be retransmitted (which causes delay). But time-based interleaving adds delay to accomplish this resiliency. Common interleaving delays range from 2 - 10 ms, when it is enabled. Many deployments do not enable interleaving because of the latency it adds.

G.fast is another copper-based technology (over twisted pair or coax) that can be used on very short loops (up to around 500 ft). Interleaving is not used with G.fast and the loop length and encoding mechanisms add very small latency.

Note that a 1 mile loop of unshielded twisted pair copper would add about (1 mile / (186,000 miles/second x 0.59)) x 1x10^6 microseconds/second = 9.1 microseconds of propagation delay. Delay caused by encoding and decoding (and any time-based interleaving) would be added to this.

### PON, (Barbara)

PON runs over optical fiber. The propagation delay in optical fiber is about 0.70 times the speed of light in a vacuum. 

### LTE/5G, (Barbara)

Wireless technologies defined by 3GPP have many latency components. The latency caused by the wireless physical medium (the air link between transmitting and receiving antennas) is the least of these. More significant are delays caused by signaling (messages required to set up a LTE session), by processing delays (how long it takes for LTE equipment to process signaling and messages including the time it takes to get the messages to where they need to be processed), and by contention with other traffic. A significant portion of backhaul is done using fiber, which minimizes latency over other backhaul technologies such as microwave or other wireless technologies. When fiber is used as the backhaul, its contribution to latency is largely due to propagation and serialization delay.

A goal of LTE design was to have lower latency than 3G. This was primarily accomplished through improvements to the signaling architecture.

5G has been (and continues to be) designed to be able to deliver lower latency than LTE. While much has been said about what may be possible with 5G (by moving intelligence closer to the edge, using network slicing, etc.), most of these possibilities have not been implemented or deployed. Some will only be used for special applications such as vehicular crash avoidance and will not be applied to general broadband Internet services.

One study that compared LTE to 5G in a specific deployment showed 5G had half as much latency as LTE in that deployment. This was determined to be directly related to the number of hops (and distance) traveled by LTE packets as opposed to 5G packets [see section 4.4. of http://xyzhang.ucsd.edu/papers/DXu_SIGCOMM20_5Gmeasure.pdf].

Median latency of the top three US mobile network providers in 2021 was calculated by Speedtest to be 33ms (across combined LTE and 5G networks) [https://www.speedtest.net/global-index/united-states].

### Satellite (DaveT)

	* media access, scheduling, etc.
	
### Core network links (Shamim)
Internet connectivity regardless of fix and mobile network interconnects with internet peering and exchange points with the help of telco & ISP owned fiber network that loops around central offices (CO) or headends (HE) that feed the end users connected to their respective ISPs and Mobile Network.
The Internet edge infrastructure typically sits at or near internet peering/exchange points as a gateway to all internet application before it reaches the centralized data centers over intercity optical backbone network. 

![End to End Network](images/CN_figure.png)

Fiber network owned by ISPs, Telco & ILECs and associated topologies play a large role in routing internet traffic from a source to destination. The speed of light on fiber is approximately 67% of the speed of light; that equates to 1 msec of RTT every 100km. Once the user  traffic aggregates in ISPs CO & Headends it traverses ISP and Carriers' IP/Optical middle mile and core transport network. The contribution of latency is primarily due to fiber distance in a well managed IP network. 

Internet routing is symmetric in nature. A request or call from a user may use one path to hit a server or end user however the response may come from totally different path that may be longer. To illustrate- if the user is sitting 100km away on a primary path from a server and the physical network topology has diverse path that is 200km then it is possible that user may experience up to 2 msec of latency due to asymmetric routing. Although fiber latency contribution in metro or regional level is less pronounced than ISPs access network it starts to play significant role if an in optimal routing on internet directs a traffic from a user from NY to SFO servers or SFO to LON servers in stead of serving locally.

Geolocation data is users approximate location that helps internet routing make correct decision on onboarding to internet edge and backbone. It is not out of ordinary that a user in NY is directed to Dallas or London if the geo database used by ISPs contains wrong location about users approximate location. The efforts underway to map IP prefixes with correct geolocation information via IETF RFC 8805, HTML5 and eDNS client subnet are designed to address bad routing issue however requires concerted effort from ISPs, MNOs, Content and App providers.  

With consolidation of ISP network and predictable intercity and submarine links the peering and transit network that interconnects all ISPs and Data Centers together has reached high level of maturity. The deeper edge and content delivery network with fine grain peering and capacity management has improved the predictability of internet performance at the interconnect exchange points. Modern peering and interconnect decision has seen a gradual shift from volume based internet exchange to latency based interconnect refinement. A step in positive direction!



## Buffering delays (3pgs)

In general, networking equipment needs to have the ability to buffer (queue) bursts of traffic that arrive at a rate that exceeds the rate of the output (egress) interface. This buffering capability serves a number of purposes:  
* when the egress interface is the bottleneck, it allows the existing congestion control algorithms to fully utilize that interface,  
* it allows applications to send (relatively short) bursts of packets without having to be concerned about the egress interface rates along the path, and  
* it handles the incast problem, where packets from multiple ingress interfaces in the device are destined to the same egress interface.

### Impact that senders & network protocols have on path latency (Koen)
* General thoughts
	* bottleneck rate determins burst/task delay
	* Queue size bursts produces delay on other flows
	* Stable/standing Queue size produces delay on all flows
	* “bursty” applications: where to absorb the burst delay: in the network, or in the sender application/stack
* Congestion Control (Classic TCP, BBR, TCP Prague, Delay-Based, Real-Time, LEDBAT)
	* CC adapts sending rate to bottleneck rate
		* common agreement of fair rate in shared bottleneck
		* Enforced by NW or agreed among senders
		* Input signal for congestion control (drop/delay/ACK-rate/explicit)
		* responsiveness of adapting the rate
		* impact of RTT on fair rate
		* Other rates (faster or slower)
		* handling/avoiding bursty traffic
	* CC determins the queue size in the bottleneck...
		* common agreement of queue target
		* Steered by NW/AQM or Sender/CC
		* Other sizes (longer or shorter) 
	* rate pacing and/or window limited
	* HW Offload impact/steering
	* Real-Time?


### Queuing implementations (Dave Taht, Greg)

The details of the buffer implementation can have a significant impact on the latency introduced by a piece of networking equipment, and thus on the end-to-end latency for all flows that utilize that piece of equipment.  The impact is felt most often when the egress interface has a lower data rate than the ingress interface (or ingress interfaces in aggregate), and particularly when the egress interface is the bottleneck  for one or more flows currently sharing it.  In those situations, packets will regularly queue up in the buffer, and thus cause delays.

#### First-In, First-Out (FIFO) Queues

The simplest (and most common) buffer implementation is a single first-in, first-out (FIFO) queue.  As packets arrive, they line up in this queue in arrival order, and they then depart on the egress interface in that same order.  If traffic arrives at a rate that exceeds the egress rate, the queue depth will grow, and if the arrival rate is less than the egress rate, the queue depth will shrink.

FIFO buffers typically have a set size that is determined by the manufacturer (and in some cases is configurable).  If the ingress rate of traffic exceeds the egress rate long enough, or if a sufficiently large burst of traffic arrives, the buffer will fill up completely, and the excess packets in the burst will be dropped.  This phenomenon (packet drop due to buffer exhaustion) is the predominant signal of congestion in the internet today, and is what most existing congestion controllers respond to.  

Historically, all congestion controllers responded to a congestion signal by stopping transmission and waiting until half of the packets *in flight* were acknowledged before resuming transmission. As a result, in order to achieve full utilization of the bottleneck link, it was important that the buffer in that bottleneck be sized to hold at least half of those in-flight packets.  And, since a network equipment manufacturer can't know a priori how many packets that might be, it was common (in devices that were expected to be the bottleneck, like DSL modems, cable modems, and WiFi gear) to provide as much buffering as possible, resulting in significant latency and latency variation when the link was being fully utilized.  This was referred to as *bufferbloat* [@Bufferbloat]. 

FIFOs that can be adjusted to a more appropriate size can improve the latency performance (in particular when the link is under load), but making the buffer too small will begin to impact the throughput of congestion controlled traffic.

#### Active Queue Management

Some network equipment has egress buffers that support a technology called *Active Queue Management* (AQM) that monitors the queue depth (or delay), and then sends congestion signals (either by dropping packets or implementing *Explicit Congestion Notification* as described later in this document) to try to sustain full egress link utilization while maintaining lower queuing delay than would exist in a FIFO.  There have been many different AQM algorithms developed over the years, but some the most common ones in use today are CoDel [@CoDel]  (usually as part of fq_codel, described below) and PIE [@PIE]. 

#### Flow Queuing AQMs

Some equipment implements multiple egress queues (often 1024) with each flow that is actively using the egress interface assigned to a separate queue, and a scheduler that ensures that each flow gets an equal fraction of the egress link bandwidth. This *flow-queuing* mechanism is generally also implemented with an AQM algorithm acting on each queue. The most common of such implementations is the fq_codel algorithm [@fq_codel].

## Latency contributions from endpoints (client & server) (1pg) (Dave Taht, +coauthor)
* Socket buffering & Offloads
* Head of line blocking & retransmissions
* Server resource contention
* VMs/Containers 

## VPNs and Proxied Paths (1pg) (Barbara, Sam)

VPN services (Virtual Private Networks) have become very popular in recent years, with advertisements for them appearing in mainstream media outlets. These services mostly advertize on the basis of increased security and the ability to spoof the users location. This paper will refrain from commenting on the veracity of such claims and instead focus on their effects on latency.

VPNs work by creating a tunnel between a client device (such as a laptop or phone) and a VPN server. All traffic that the client device would normally send directly to the Internet is instead redirected via the VPN tunnel. Traffic then leaves the VPN server and reaches the Internet. This means that that the user's source IP address appears to be that of the VPN server, which provides the stated location spoofing capabilities. All traffic through the tunnel is typically encrypted, which provides the stated security improvements.

### VPNs and latency

In terms of latency, there is almost always a latency penalty when utilizing VPNs. The most obvious latency penalty is the increased length of the path between the user's device and the internet. Without the VPN, the user's traffic passes over the ISP's access network, to a peering location, and then reaches the wider internet. With the VPN, the user's traffic passes over the ISP's access network, to a peering location, then over one or more intermediate networks until it reaches the VPN server, and then to the internet. The location of the VPN server therefore becomes very important to latency. If the VPN server is very close to the ISP's peering location and the user's destination on the internet, then the latency penalty may be minimal. If not, then the latency penalty could be much more significant.

Many VPN services allow the user to choose the location of the VPN server used (thus allowing the user to appear to be in a different location). Such uses will always incur a latency penalty that is at least as great as the latency between the user and the spoofed location.

VPNs can have less obvious impacts on latency too. Many ISPs have interconnection relationships with CDNs, such as Akamai, Google, Netflix, Facebook and so on. Sometimes this even involves the CDNs installing caches inside the ISP's network. CDNs use the source IP address of the user (or a portion of it) to work out where the user is and what ISP they are on, which is then used to steer the user towards a CDN server that is optimal for the user's location and ISP. When operating over a VPN, the user's true IP is hidden, so the CDN does not have the information necessary to steer the user towards the optimal CDN server. This means that a user on a VPN will likely experience increased latency to CDNs, because the optimized path to the CDN cannot be utilised.

Finally, the VPN client and server itself may introduce additional latency, particularly if the VPN server is over-utilized.

### iCloud Private Relay (this section to be rewritten as HTTP Relay more generally by Barbara & Shamim)

In 2021, Apple announced a new privacy feature that would be available to subscribers of its premium service. This feature, called iCloud Private Relay, is similar to the VPN services discussed above but with a few differences. The key difference is that traffic is distributed across multiple tunnels and exits onto the Internet across multiple servers in a nearby location. This has the stated benefit of providing enhanced privacy, by removing the ability for someone eavesdropping at a single VPN server to have visibility of a user's complete traffic.

Based upon the publicly available information, iCloud Private Relay will have a similar impact on latency to other VPNs. The selection of Cloudflare, Fastly and Akamai as partners to host the exit nodes (equivalent to VPN servers, where traffic exits to the internet) means that there will be many more VPN termination locations than most services. This will likely help reduce the latency penalty, but it certainly will not remove it. If appropriate client context is supplied when resolving DNS queries, it may resolve the issue of CDNs being unable to steer traffic to the most optimal server for the user.

# Current and Future Technologies to improve latency performance (3pgs) (Shamim)

## Migration to the network edge (CDNs, MEC) (Shamim)
Internet edge network is constantly evolving to suite the optimal user experience needs. For example a video streaming is throughput sensitive and maintaining optimal buffer on the end device or access point is desirable where as a real time video call may require lower throughput but extremely sensitive to higher buffer and packet loss. 

Cloud gaming and edge compute on the other hand may require very low latency, predictable throughput and packet loss. Internet traffic has traditionally been highly asymmetric and served really well for download heavy content delivery however the dawn of cloud compute and storage has proven that uplink from user to cloud traffic needs immediate attention. More and more internet applications depend on internet edge for cloud user experience requiring lower latency, predictable throughput and packet loss in the uplink path. More and more applications implementing multi path using broadband and 5G network, for example to reach the local internet edge to mitigate latency, throughput and loss. Multi path implementation is an optimal use of access network resource however would require carrier neutral host for edge compute and proxy that enables users to select more than one ISP before it hits local internet edge. Consumer and small businesses can immensely benefit from neutral host network edge compute aka NEC. On the other hand internet experience is better with on premise compute for large enterprise, malls or arenas using multi-access edge compute, aka MEC. 

To recap- Edge compute requires higher level of SLO from access network. Multi-path implementation on application unleashes the potential of access network connectivity options and elegantly solves capacity and performance concerns. Neutral host compute with access to more than one ISP brings a new potential to accelerate low latency services. 

## Local caching (DNS, etc.) (Cullen, Shamim)

## Traditional Quality of Service differentiation (Greg)

One mechanism used in some networks to manage latency performance is the differentiation of traffic using traditional Quality of Service (QoS) techniques.  Most networking gear, from inexpensive home routers to access network equipment to high performance switches used in carrier networks and datacenters, supports features that can treat packets differently via some configurable criteria.  [@BITAGdifferentiation] provides a detailed treatment of this subject, but for the purposes of this paper the topic can be summarized briefly.  

QoS differentiation involves identifying application traffic flows based on business or technical factors, and then treating the different traffic flows differently within a network device.  In general, differentiation only has an impact in the network devices that experience congestion.  Congestion is a normal part of the design of the Internet, but not all network devices experience congestion, so proper configuration of QoS policies involves understanding network congestion points, and ensuring that the QoS policies at those congestion points provide the desired treatment.

QoS differentiation is commonly used within enterprise networks and to differentiate between specialized services in carrier networks. It is generally not feasible to utilize end-to-end for Internet traffic.  QoS management (both at the technical level and at the policy level) is complex.  Frequently, traffic identification involves determining the subjective latency/loss sensitivity and/or importance of a particular traffic aggregate, and the tools available within network equipment often amount to simple prioritization between service classes.  The result is that the more important and/or QoS sensitive an application is believed to be, the higher priority it is given, thus creating a zero-sum game where degradation of one category of traffic is reduced at the expense of another. This necessitates the use of access controls and policing to ensure that applications aren't able to game the system by gaining access to a higher priority level than allowed.  This is complex enough to manage across multiple applications and multiple users within an enterprise network, and it becomes infeasible to manage across the multiple networks that make up the Internet.

Along the edge of the internet, extensive options for QoS are often available, including but not limited to, rate shaping, per-device or per-application (de)prioritization, and parental controls.

## Explicit Congestion Notification (Greg)

Historically (and still commonly today) the Internet has used packet discard (drop) as a means of signaling congestion to endpoints, and endpoints, in turn, have used the detection of packet drops as a way to sense congestion and thus modulate the rate at which they send traffic into the network.  Endpoints additionally need to recover from the loss of data by the network, either by concealing the lost data (e.g. in the case of a real-time voice call), recovering the lost data (e.g. using Application-Layer Forward Error Correction), or retransmitting the data (e.g. in the case of TCP).  Thus packet discard is both a congestion signal and an impairment for the endpoint application, and as a result the network needs to be judicious in its use.

Twenty years ago, a 2-bit field in the IP header, known as the Explicit Congestion Notification field, was defined [@RFC3168] in order to allow networks to signal congestion to endpoints explicitly without the need to drop packets.  This technology eliminates the impairment aspect of the congestion signal, and thus can provide a latency benefit in that packets no longer need to be retransmitted, and thus the latency of the congestion signaling packets is reduced by one full RTT.  Further, since many applications require data to be delivered in order, ECN eliminates the "head-of-line blocking" phenomenon where later packets are delayed at the receiver awaiting the arrival of the retransmitted packet.

Alas, adoption of ECN has been relatively low.  While many endpoint protocols support it, there are not many networks that do.  A recent study by Akamai [@Akamai] concluded that globally, around 0.19% to 0.30% of ECN-capable clients ever saw an explicit congestion signal over the course of a day.

In its current definition, explicit congestion signals are sent as judiciously as packet drops are, the result being that the network needs to tolerate a relatively high level of congestion. Five years ago, it was recognized that the definition of ECN missed an opportunity.  Since the congestion signal is no longer an impairment, the need to be judicious about its use goes away, and the network could provide much more fine-grained feedback about congestion to endpoints.  Additionally, it was observed that one of the four values that can be encoded in the ECN field had gone unused and could be used to enable the definition of a new version of ECN.  This is the subject of the next section.


## QB/NQB distinction & Low-Latency, Low-Loss, Scalable throughput architecture (Greg)

The IETF has recently defined a technology [@L4S] that enables applications to adjust their sending rates to make full use of the bottleneck link in a fair manner without causing the latency and latency variation that existing applications do.  This technology, referred to as "Low Latency, Low Loss, Scalable Throughput" (L4S) is an evolution of the Explicit Congestion Notification technology discussed in the previous section.  It utilizes the as-yet-unused value (referred to as ECT1) in the Explicit Congestion Notification field in the IP packet header to allow senders to identify themselves as supporting L4S. Then, an L4S-capable bottleneck link can use the existing *Congestion Experienced* value to send immediate signals whenever a queue begins to form.  This signal, when fed back to the sender via acknowlegement packets, enables the sender to adjust its sending rate in order to keep the bottleneck link busy without building a queue.  L4S is incrementally deployable, since senders will always need to handle other congestion signals (like packet drops) that arise from bottlenecks that don't support L4S.  In addition, the L4S architecture requires an L4S bottleneck to isolate the L4S-capable traffic from the non-L4S-capable traffic (referred to as *classic* traffic) via separate queues, so that the queuing delay caused by the classic traffic doesn't impact the L4S traffic.  

Along the same timeline as the development of the L4S specifications in IETF, it was recognized that there are some applications that don't seek to send data at the fastest rate possible (i.e. they are not *capacity-seeking*), but rather they send at a relatively low data rate, and thus they don't materially contribute to queuing delay and packet loss in the network.  Nonetheless, these applications (many of which are latency and loss sensitive) are subjected to the latency, latency variation and loss caused by other senders that do cause these degradations. This category of traffic is referred to as *Non-Queue-Building* (NQB) traffic.  Good examples of NQB applications are Voice over IP applications (including the audio stream of video conferencing applications), multiplayer online games and DNS lookups.  The IETF is developing [@NQB] a standardized way for such applications to identify themselves to the network, and requirements for isolating the two classes from one another such that both can share the bottleneck link capacity.

Networks that support both of these technologies can enable applications to share a bottleneck link and provide a net benefit in latency and loss performance for the applications that aren't causing those degradations, without negatively impacting the performance of the remaining applications.


# Metrics and methods for characterizing latency performance (5pgs) 

## Latency for a path in a live network is variable — a statistical distribution (Greg)

Characterizing the latency performance of a network path from one machine to another generally involves sending one or more packets along the path, and calculating the time that it takes for those packets to arrive at their destination.  When measuring one-way latencies, this requires both endpoints to have synchronized clocks.  The sender inserts a timestamp into the packet that it transmits, and when the packet arrives at the receiver, the receiver checks its local clock and compares it to the timestamp value in the packet.  

It is more common to measure the round-trip time (RTT), since this can be done even in the absence of synchronized clocks, and for most applications the round-trip time is more important anyway.  In this case the sender typically sends a packet to the receiver, the receiver sends a response packet back to the sender, and the sender then calculates the time elapsed between transmission and reception. 

In both of these cases, each measurement represents a single sample of the latency along the path. However, it is typical in many real-world environments for the latency of a path to vary from one measurement to the next, sometimes considerably.  So, it is usually advisable to collect multiple measurements and then summarize the outcomes of the ensemble of measurements statistically.  It is also frequently the case that the statistics of latency will vary over time as the load on the network changes, the path itself changes, etc. 

Historically, the most common descriptive statisic that is reported (and often labeled simply as "latency") is the average latency of the ensemble of measurements.  Other descriptive statistics that are sometimes reported are the standard deviation of the samples, the minimum value of the samples, and the maximum value of the samples.  The term "jitter" is often used in some way to refer to the variation of latency from sample to sample. 

For many paths, the distribution of latency samples does not take the form of a traditional Gaussian (Normal) distribution, where mean and standard deviation completely describe the distribution. As a result, the two metrics of average and standard deviation are often not particularly useful to understand the latency characteristics of a path.  Rather, it is very common that the minimum latency and the average latency are fairly close to one another, and there are large "spikes" in latency due to various network phenomena. These latency spikes can be relatively rare, yet can be a significant factor in determining the quality of the path and its suitability for a particular application.  The effect that latency variation on application QoE is described further in Section XX.

Since latency variation affects different applications differently, there have been multiple attempts to define metrics that can describe the variation in a way that correlates to the impact it has.  Each of these definitions are typically referred to as "jitter", even though they may vary considerably in their definition. [https://www.nctatechnicalpapers.com/Paper/2020/2020-latency-measurement] describes five different definitions of jitter that are in common use in the industry.  Calculating each of these metrics on a particular set of latency measurements can result in "jitter" values that differ by a factor of 100 or more.  So, when using jitter metrics, it is important to be clear which definition is being used, and to consider whether that defintion is meaningful given the application context. 

Another approach to characterizing a set of packet latency samples is to use order statistics, e.g. minimum, 25th percentile ("P25"), median (P50), P90, P99, maximum.  This approach can be particularly useful when used with isochronous applications like voice communication and multiplayer online games, since these applications commonly employ a "jitter buffer" that converts latency variation into fixed latency and residual packet loss (described further in <xref>).  So, as an example, a jitter buffer that results in 1% residual packet loss would mean that the application is operating with a fixed latency equal to the 99th percentile, and thus the measured P99 latency would be a strong indicator of the quality of the connection for this application.  Since many such applications are likely to target low values of residual loss (e.g. 0.1% to 5%), latency percentiles in the range of P95 to P99.9 may be the most useful in predicting quality of experience.

When possible, a more complete view of the latency statistics of a path can be had by plotting the full statistical distribution from the measurements.  One of the most useful representations is in the form of a Complementary Cumulative Distribution graph, plotted on a log scale, with the axis labeled to represent packet latency percentiles.  An example is shown below (TODO, generate higher quality image).

![Complementary CDF of Packet Latency](images/ccdf.png)

Another metric that has been proposed recently, and that derives from latency, is "responsiveness" [https://www.ietf.org/id/draft-cpaasch-ippm-responsiveness-00.html].  This metric consists of more than just packet latency measurements, but rather is based on higher-layer protocol latency measurements including DNS, TCP handshaking and HTTP.  These measurements are averaged, and then the result is inverted and expressed in units of "Round-trips per Minute" (RPM).  This metric has some intuitive value for iterative web traffic workloads, where a user can imagine a network with low responsiveness setting an upper bound on how many web resources can be fetched in a certain amount of time.   That said, it is specific to web workloads, and appears to focus on the average result of a small number of measurements, as opposed to trying to represent information about the range of performance that the user might experience.


## Measuring latency (Sam)

There are many different protocols that can be used to measure latency. For each of those protocols there are often multiple tools that provide measurement capabilities. The list below is by no means exhaustive, but covers some of the most common techniques in use today.

### Protocols

#### Ping (ICMP)

The most well known one is the ping command. This is built into all major operating systems and was first introduced in 1983. Ping uses ICMP echo packets to measure round-trip latency and packet loss. Example output from pinging ietf.org can be found below:

```
[sam@localhost ~]$ ping ietf.org
PING ietf.org (4.31.198.44) 56(84) bytes of data.
64 bytes from mail.ietf.org (4.31.198.44): icmp_seq=1 ttl=53 time=167 ms
64 bytes from mail.ietf.org (4.31.198.44): icmp_seq=2 ttl=53 time=170 ms
64 bytes from mail.ietf.org (4.31.198.44): icmp_seq=3 ttl=53 time=165 ms
^C
--- ietf.org ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2002ms
rtt min/avg/max/mdev = 165.472/167.446/169.641/1.709 ms
```

The operation of ping is quite simple. The command will send repeated 'pings' (ICMP Echo Requests) to a destination, and wait for the replies (ICMP Echo Reply). If a reply is received, it prints the time that elapsed between sending the request and receiving the reply. If a reply is not received within a reasonable timeout, then it prints that the packet was lost. Most networked devices will respond to pings (ICMP echo requests) by default. This means that no special software or configuration is required to use ping at all. The ubiquity of ping in major operating systems and its easy-to-setup nature means that its use is still very common today for ad-hoc measurement and troubleshooting.

#### UDP

UDP is a common choice for applications that need to measure latency. UDP does not have any built-in reliability guarantees, and therefore no automatic retransmission of packets that are lost or corrupted. Avoiding such things is desirable in latency measurement, because to do otherwise would introduce external factors into our measurement (such as artificial delays before retransmitting a lost packet) that cannot be reliably separated from the network latency. There's a large variety of tools that use UDP as their basis for latency measurements.

The IETF has standardized multiple UDP-based latency measurement protocols over the years. OWAMP (one-way active measurement protocol) provides for one-way latency measurements between sender and receiver. This means that the latency between the sender and the receiver in the forward direction is measured and reported separately to the latency between the sender and receiver in the reverse direction. This provides valuable information that is lost in two-way (round-trip) latency measurements — it can show if latency in one direction is larger than the other. An impediment to adoption of OWAMP is the requirement that the sender's and receiver's clocks are precisely synchronized. Relying on NTP[^3] alone is usually not sufficient here, as the precision is not high enough for low latency connections.

[^3] Network Time Protocol, a commonly utilized protocol to automatically set the time in a network-connected machine. 

TWAMP (two-way active measurement protocol) extends OWAMP to also support two-way (round-trip) latency measurements. When being used only for round-trip measurements, the requirement for the clocks on the sender and receiver to be synchronised can be removed. This is because the measurement is only conducted at the sender — the receiver effectively just has to reflect the packet back to the sender. TWAMP is often deployed inside large routers from companies like Juniper and Cisco for the purposes of service level agreement verification. It is favoured over ICMP because it supports separating out host-processing latency from network latency.

STAMP (simple two-way active measurement protocol) simplifies TWAMP by removing some little-used features, whilst still maintaining backwards compatibility with the existing TWAMP protocol.

SamKnows, a UK-based provider of network measurement services, have deployed a proprietary UDP-based latency measurement protocol. This supports round-trip measurements only, but is otherwise quite similar to the other UDP approaches discussed above. The SamKnows UDP latency measurement tool supports persistent operation, reporting latency and packet-loss statistics at a configurable interval, rather than measuring for a short period and then finishing. It also has built-in cross-traffic detection and latency-under-load capabilities, which are discussed further below.

#### TCP and HTTP/1.1

TCP can also be used to measure latency. It is a less common choice for network measurement applications.  TCP's reliability mechanisms effctively sacrifice latency for the sake of reliability. This introduces some ambiguity in the measurement results — we cannot know whether the latency being reported is due to the network path or some feature of TCP (e.g. retransmission of lost segments).

However, there is a counterargument: If many applications are based upon TCP, then using TCP based latency measurements will better correlate to end-user experience. Of course, this descends into a philosophical question of what are we trying to measure — network path latency or user experience. TCP is better suited to the latter (at least for TCP based applications), but it is still just a middle ground — a real application-level user experience latency measurement would be interleaved with the application traffic itself.

There are a few different ways to measure latency over TCP. A simple and common choice is to simply measure the duration of repeated TCP three-way handshakes. The connect() system call in Linux is commonly used for this. This has the advantage that it can be used to measure TCP round-trip time to any host that has a listening TCP server.

Another approach, commonly used by web-based applications such as web based 'speed tests', is to make repeated HTTP HEAD requests to a web server. Web servers will often use persistent HTTP sessions (via the HTTP Keep Alive feature), which allows the application to avoid measuring the overhead of establishing the underlying TCP connection. This approach is often used by web-based applications because of the sandboxing restrictions put in place by web browsers — they are prevented from exchanging arbitrary UDP traffic and are often limited to HTTP or WebSockets.

#### HTTP/2 Ping

Unlike its predecessor, HTTP/2 provides built-in 'ping' support via the PING frame type. This is intended to be used for round-trip time measurements and also to keep the connection alive.

Measuring latency using HTTP/2 pings (often abbreviated to h2ping) has one key advantage over techniques discussed previously. By interleaving the latency measurement with the real application traffic using the same protocol (HTTP/2), we remove the possibility of our latency measurements being treated differently from application traffic. This means that we can have a greater degree of trust that our latency measurements conducted using HTTP/2 Pings accurately represent the latency experienced for HTTP/2 based applications.

Of course, with HTTP/2 being delivered over TCP, we still have the fundamental ambiguity over whether delays are in the network path or are being introduced by one of the host's TCP stacks.

### Awareness of network conditions for latency measurement

Network conditions can have a significant impact upon latency. A completely idle network can have a very low and consistent measured latency (using any of the tools and protocols above). Conversely, if the network is heavily in use, then latency can increase significantly and become very erratic. Therefore it is important with latency measurements to understand the conditions in which you are measuring.

#### Cross traffic

Traffic from other users and applications that share the same Internet connection is called 'cross-traffic'. Some measurement systems will measure cross-traffic levels before carrying out latency measurements, and may defer them or cancel them entirely if cross-traffic has exceeded a tolerable threshold. An example of this is the SamKnows solution, which uses a 64kbit/s threshold for cross-traffic tolerance by default. An alternative approach is to allow the measurement to proceed, but to report the cross-traffic levels alongside the measurement results, therefore allowing them to be studied or filtered later.

#### Idle latency and latency under load

Latency measurements that are carried out in the presence of little or no cross-traffic are said to be measuring 'idle latency'. This is useful to work out the baseline latency of a network path.

It is also useful to carry out latency measurements in the presence of heavy cross-traffic. Carrying out latency measurements under such conditions is known as a 'latency under load' test. This helps to reveal how latency behaves when the network is heavily utilised, which is precisley when users are using it. Some measurement systems will generate artificial cross-traffic (perhaps in the form of a throughput test) in order to ensure the link is heavily utilised to a predictable and repeatable degree. Measurement systems that do not measure cross-traffic cannot reliably know whether they are measuring idle latency, latency under load, or something in between.

# How do latency and latency variation impact user experience? (incl. mention of latency mitigation techniques) (5pgs)

   * Mean Opinion Score
        * problem with measuring just recorded media quality not overall
        UX or QoE
        
## VoIP and Video Conferencing (Cullen, Dave Taht)

Voice and video conferencing systems are one of the Internet’s most used features. They are used for meetings between workers — both inside and across companies, for education and teaching, and to connect friends and families. In the US alone, there are over **TODO** minutes of meetings per month.  All of these systems are highly sensitive to latency.

`This paragraph needs tightening:` The "microphone to speaker" delay is measured from the time the audio is recorded at the microphone of the participant, until the time that same sound plays out on the speaker of the other participants. When the delay is low, the call or meeting can seem like a normal conversation. As the delay gets longer, it becomes harder to have a conversation. Two users will both try and speak at the same time and end up talking over each other. This is because the delay means that each user cannot tell that the other user was already speaking. Many people have experienced the effect where two people speak at the same time, then they both stop and tell the other to go, then they both go at the same time again. This does not happen on low latency meetings but is common on higher latency meetings. When the latency is too high, one side will say something, then experience an unnatural silence when the other side does not respond. When a remote person is slow to respond, humans sometimes assume they are not as smart as a person that responds quickly. This raises the question about whether or not this has any unconscious bias impact on teachers who have students on both low latency and high latency network connections. 

Low "microphone to speaker" delay for audio, and related “camera to screen” delay for video, are critical for a good user experience on a VoIP (Voice over Internet Protocol) call or video conference. There are several things that contribute to this latency:

*	Capture buffer latency: the audio needs to be recorded by the hardware of the computer and passed as a chunk of information to the program.  
*	Encodings: audio and video are grouped and compressed so that it can be sent over the network, but this requires waiting for an appropriate amount of data to group together. This is referred to as the encoding delay.  
*	Network latency: the time for the media to be transferred over the Internet.  
*	Media server latency: time for cloud servers that distribute and process media to forward it and sometimes encode, decode, and remix it.  
*	Jitter buffers: some media will be delivered faster than others and the receiver has a buffer to save things that arrived early and play them at the appropriate time.  
*	Forward error correction: time to allow for receiving extra information to replace lost packets.  
*	Retransmission: time to allow the request and receipt of another copy of packets lost by the networks.  
*	Playout buffers: queue the media to be played by the hardware of the computer.  

### Jitter

When voice and video media packets are sent across the network, they will not all have the same latency, some will arrive faster than others. However, the media needs to be played out at a constant rate that matches the rate at which it was recorded. VoIP applications buffer a small amount of media to smooth over these variances in arrival times, which is referred to as jitter. If 95% of the packets take over 30 milliseconds to arrive, it does not matter if the average latency is 20 milliseconds because the VoIP applications will delay all the packets so that they take the same amount of time as the slower 30 millisecond packets. The result of this, is that for VoIP applications, average latency plus the amount of jitter is what determines how much delay is caused by the network. The amount of jitter is just as important as the latency to the overall experience that the user has.  A network with an average latency of 50 milliseconds where less than 10% of the packets have a jitter higher than 40 milliseconds, will usually have more of a “speaker to microphone” delay than a network with an average latency of 60 milliseconds where less than 10% of the packets have a jitter higher than 5 milliseconds. 


### Forward Error Correction

Many VoIP applications use a range of techniques to recover from losing packets, that involve sending some of the packets twice or sending extra information about groups of packets that allow an application to reconstruct the information from a lost packet. Packets are often lost in small groups. To recover the lost packets, the information to recover them cannot be lost, so it needs to be transmitted far enough from the original packets so that it is less likely to land in the same loss group. This inherently means that the forward error correct adds in more delay than the size of commonly observed loss groups. Networks that lose packets in groups, tend to have a longer “microphone to speaker” delay than networks that are very random in which packets they lose, and do not have correlated losses.

For very short segments of lost media, audio, or video, it is often possible to "fake" the media by looking at the media immediately before and after it. This can deal with short losses but also adds a delay to look at the media after the parts that were lost. 

### Retransmission

Another way that VoIP applications can compensate for packet loss is by requesting the retransmission of the lost packet. The receiver needs to wait for an amount of time equivalent to the normal network latency plus the jitter time before the packet is requested. Then the request to retransmit the lost packet must cross the network to the sender and the sender can resend the lost data to the receiver. This takes around three times the normal delay to cross the network. If this technique is being used, all the packets that are not lost also need to have their time to be played out, so they can be played with the correct timing for the packets that were retransmitted. The key thing to note here is that a 10 ms increase in network latency can cause a 30 ms increase in “microphone to speaker” delay.

### Quality of Experience

For Internet voice and video conferencing and calling systems, the network latency is the major factor causing large “camera to screen” delay and “microphone to speaker” delay. The network latency contributes to the delay, but other aspects of the network also contribute. Packet loss rates and the grouping of packet loss have a large impact on the overall delay. The delay has a huge impact on how well people can communicate.


## Multiplayer online games (Alex)

`This section needs tightening`

Multiplayer online games are particularly sensitive to latency. This is especially true for fast-paced games, such as first person shooters (FPS) or sports games. Even in the early days of such games in the late 1990s, gamers were acutely aware of the impact of latency on their experience.

For the avoidance of doubt, we are concerned only with gameplay here, which is highly latency sensitive. Modern games will also download game updates, content for future parts of the game (videos, animations, maps, etc), which are bulk downloads and generally not immediately visible to the user, so are less latency sensitive.

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

The past few years has seen the emergence of 'cloud gaming' platforms, such as Google's Stadia or nVidia's GeForce Now. These move the graphics intensive work to the server side and then effectively transmit the video of the gameplay back to the client side. This allows the game to run on far lower specification devices, without the need for powerful GPUs client side. The developers of these platforms say that increases in Internet access bandwidth and decreases in latency make this approach possible.

This, of course, means that cloud gaming platforms are now more bandwidth intensive than traditional games, as they have to transmit high resolution video at a high frame rate to the client side. GeForce Now, for example, states that it "requires at least 15Mbps for 720p at 60fps and 25 Mbps for 1080p at 60fps".

There is also anecdotal evidence that suggests that high or erratic latency may also be used by cloud gaming platforms as a signal to reduce the resolution of the game. Little published material is available from the cloud gaming platforms to explain their approach in technical detail.

All of the issues discussed in the above multiplayer online gaming section are true for cloud gaming too. Plus, there are some interesting additional issues to consider. In the previous section we discussed that the client can apply its own local movement updates in order to show immediate feedback to the local player (even if the remote players will not perceive it for some milliseconds yet). The ability to do this disappears with cloud gaming. All of the inputs must be sent to the server side, processed, and then transmitted back to the client for display. This can lead to a player pressing the shoot button, and then not seeing that take effect on screen until a short time later. This leads to a significantly higher 'controller-to-photon' delay than with a local-only game.

An example of this phenomenom can be seen in a PC Gamer article at https://www.pcgamer.com/uk/heres-how-stadias-input-lag-compares-to-native-pc-gaming/




## Web browsing (Peter)

Here is some information relevant to (perceived) latency of web browsing.

A few years ago, Google started a project called Web Vitals, which
defines a number of measures:

https://web.dev/learn-web-vitals/

The Firefox team tends to rely most heavily on a measure called
First Contentful Paint, which in our experience is slightly more
representative of perceived latency than Largest Contentful Paint:

https://web.dev/fcp/

https://developer.mozilla.org/en-US/docs/Glossary/First_contentful_paint

FCP is sometimes also called First Visual Change:

https://www.sitespeed.io/documentation/sitespeed.io/metrics/#visual-metrics

There are many, many factors involved in latency and responsiveness. To
give you a flavor, here is a recent blog post from the Firefox team:

https://blog.mozilla.org/performance/2021/07/13/bringing-you-a-snappier-firefox/

Naturally these things vary across browsers, so if we write a few
paragraphs about these topics we'll want to make sure they apply across
the board.



## Future applications

As noted above, consistently reducing working latency will improve all existing user applications. But looking to the future, it seems likely that the emergence of very low latency services may enable entirely new classes of applications to be created. One way to think about this is to consider that today we assume accessing resources on the Internet has some delay compared to local content or applications on a device. But what if a network-based resource was as responsive as a locally-installed resource? As well, applications that today are infeasible without highly specialized private Internet connectivity might become viable over consumer-grade best efforts Internet access. What follows are some applications that may be viable, though it is likely that unexpected and surprising new applications will be released by creative developers that we are unable to envision or list in the brief examples below. 

### Connected cars (Barbara)
	
Major connected car applications that have low latency requirements are self-driving / autonomous cars and crash avoidence mechanisms. None of these rely on networked solutions to work at this time. Instead, they currently rely on local sensor information. For networked mechanisms to be useful for these applications, latency will need to be about 1 ms. This will only be achievable if the connectivity is directly between vehicles or if packets between a vehicle and something in its vicinity traverse just a single, nearby hop.  
	
### Cloud VR (Greg)

Virtual reality environments require extensive compute resources in order to render high quality scenes with sufficient frame rate. These requirements have motivated the exploration of cloud-rendering solutions, where the cost of the compute resource can be shared across a number of users. A key consideration in moving the majority of the rendering functionality to the cloud is the latency between a movement of the user's head, and appropriately updated images being presented to them in their head-mounted-display.  This *motion-to-photon* latency can be no more than about 20 ms otherwise it can cause nausea, with some targeting less than 8 ms for the ideal experience [@CloudVR]. For full remote rendering, much of this latency budget is consumed by the motion-capture and image rendering processes, leaving perhaps 1-2 ms of network RTT between the head mounted display and the rendering engine. 

# Conclusions/observations/findings

* There will always be a constrained link on an Internet path, due to its packet-based design which means in part that it does not create end-to-end circuits with dedicated capacity.  
* The fact that some packets are lost at the bottleneck is also completely normal and indeed is essential, as this is a key part of the control signal or feedback loop to a sender to moderate how fast they are sending packets.  

* The location of the VPN server therefore becomes very important to latency.
* Latency percentiles in the range of P95 to P99.9 may be the most useful in predicting quality of experience.  

# Recommendations

* Recommendations for networks and applications	(author?)
* OS developers
* CPE manufacturers 
* etc


To really understand end-user QoE, we need to look at working latency rather than idle latency.  
When using jitter metrics, it is important to be clear which definition is being used, and to consider whether that defintion is meaningful given the application context.  


\pagebreak
# Heading Level 1

aldkfja;dklfja;dklfja;ldkfja

example of reference [@aca1]

Example footnote marker [^1]

Example footnote text 

[^1]: Put the text for footnote here


## Heading Level 2
### Heading Level 3
