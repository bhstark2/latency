
* Introduction (3pgs) (Jason)
	* Effect on Applications
		* User experience and lag in web browsing, web conferncing, and gaming 
		* How latency impacts UX 
	* Mention simple test for buffer bloat / lag
	* Trends of User Experience and Network Performance
	* Why latency matters
	* Common misconceptions 

* Definition of Latency (1pg) (DavidReed, Jason)
	* Latency as a property of a network path (0.5pgs)
		* Points A & B, and the network path that connects them
		* Latency is additive along a path – contributions from multiple sources
	* Latency as a property of a network condition (0.5pgs)
		* Idle latency vs Working latency
	* Impact that data rate has on latency
		
* Sources/Contributors to Latency (8pgs)
	* Link technologies in place along the path (3pgs)
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
	* Buffering delays (3pgs)
		* Impact that senders & network protocols have on path latency (author?)
			* Congestion Control (Classic TCP, BBR, TCP Prague, Delay-Based, Real-Time, LEDBAT)
			* Other “bursty” applications
		* Queuing implementations (Dave Taht, Greg)
			* FIFOs (buffer sizing)
			* AQMs: CoDel, PIE, DOCSIS-PIE, Cobalt, etc.
			* Flow queuing: fq\_codel, fq\_pie, CAKE
	* Latency contributions from endpoints (client & server) (1pg) (Dave Taht, +coauthor)
		* Socket buffering & Offloads
		* Head of line blocking & retransmissions
		* Server resource contention
		* VMs/Containers 
	* VPNs and Proxied Paths (1pg) (Barbara)
		* iCloud/Safari Private Relay, etc.

* Current and Future Technologies to improve latency performance (3pgs) 
	* Recommendations for networks and applications	(author?)
	* Migration to the network edge (CDNs, MEC) (Shamim)
	* Local caching (DNS, etc.) (author?)
	* Traditional Quality of Service differentiation (Greg)
	* Explicit Congestion Notification (Greg)
	* QB/NQB distinction & Low-Latency, Low-Loss, Scalable throughput architecture (Greg)
	
* Metrics and methods for characterizing latency performance (5pgs)
	* Latency for a path in a live network is variable – a statistical distribution (Greg)
		* Descriptive statistics: Average latency, min/mean/max/std
		* Order statistics (P0,P99,P99.9)
		* Packet Delay Variation vs. Inter-Packet Delay Variation vs. “Jitter”
		* CDFs
		* "Responsiveness" RPM (round-trips per minute)
	* Measuring latency (author?)
		* Protocols: ICMP (PING), UDP (STAMP, TWAMP, SamKnows, etc), TCP, HTTP/2 Echo
		* Awareness/manipulation of conditions (i.e., monitoring cross-traffic, introducing cross-traffic, etc.) 
			* Idle latency
			* Working latency
				* Latency Under Load
* How do latency and latency variation impact user experience? (incl. mention of latency mitigation techniques) (5pgs)
    * Mean Opinion Score
        * problem with measuring just recorded media quality not overall
          UX or QoE 
	* VoIP, video conferencing (Cullen, Dave Taht)
        * contributing source so speaker to microphone / glass to glass
        latency
            * capture buffers, encoding, network, media servers,
              retransmission, error correction, jitter buffers, play-out
              buffers 
	    * Impact of latency
              * call not feeling interactive 
		      * users "talking over" other users
              * perception of other not understanding or not being as
                smart due to lag in others peoples responses 
		* Jitter buffers and how jitter increases microphone to speaker
        latency
              * what jitter is ( can this be explained before this
              section ??? )
              * how VoIP applications deal with jitter
              * why packets with high jitter, not the average jitter,
                determines the impact to latency
              * example that 50 ms latency with a 90% jitter of 40
                ms is much worse than 70 ms latency and 5 ms of jitter
                even though the average latency is less for the first
                case
        * Packet loss patterns and impact on latency
              * Dealing with packet loss by redundant encoding and FEC
              * length of burst packet lost and impact on latency
              * packet concealment for short losses 
        * Retransmissions request and impact on latency
              * how VoIP retransmissions works 
                  * jitter impact on how soon to request the retransmission
                  * multiple RTT impact on latency
                  * why this is used for video reference frames
                  * lipsync and impact on audio
        * Key points
              * interactive voice and video QoE is strongly dependent on
               low glass to glass latency
              * jitter adds to glass to glass latency
              * packet loss adds to glass to glass latency
              * packet loss patterns, particularly burst loss, impacts
                glass to glass latency
              * for voice calls with a good QoE, the packet loss
                patterns, latency, and jitter are nearly always more
                important than how much bandwidth is available
	* Multiplayer online gaming (Alex)
		* Lag compensation techniques
		* Jitter buffers
	* Cloud gaming (Alex)
		* Controller-to-photon latency 
	* Web browsing (Peter)
		* Page Load Time & other metrics
	* Future applications
		* Autonomous cars (Barbara)
		* Cloud VR (Greg)
		* Remote Surgery (Dave Taht)
* Conclusions & Recommendations
