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
