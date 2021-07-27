
* Introduction (3pgs)
	* Effect on Applications
		* User experience and lag in web browsing, web conferncing, and gaming 
		* How latency impacts UX 
	* Mention simple test for buffer bloat / lag
	* Trends of User Experience and Network Performance
	* Why latency matters
	* Common misconceptions 

* Definition of Latency (10pgs)
	* Latency as a property of a network path (0.5pgs)
		* Points A & B, and the network path that connects them
		* Latency is additive along a path – contributions from multiple sources
	* Latency as a property of a network condition (0.5pgs)
		* Idle latency vs Working latency
	* Link technologies in place along the path (2pgs)
		* WiFi, DOCSIS, DSL, PON, LTE/5G, Ethernet
			* media access, scheduling, etc.
		* Core network links
			* Propagation velocity, path distance, path stretch
	* Buffering delays (3pgs)
		* Impact that senders & network protocols have on path latency
			* Congestion Control (Classic TCP, BBR, TCP Prague, Delay-Based, Real-Time, LEDBAT)
			* Other “bursty” applications
		* Queuing implementations
			* FIFOs (buffer sizing)
			* AQMs: CoDel, PIE, DOCSIS-PIE, Cobalt, etc.
			* Flow queuing: fq\_codel, fq\_pie, CAKE
	* Current and Future Technologies to improve latency performance (3pgs)
		* Recommendations for networks and applications	
		* Migration to the network edge (CDNs, MEC)
		* Traditional Quality of Service differentiation
		* Explicit Congestion Notification
		* QB/NQB distinction & Low-Latency, Low-Loss, Scalable throughput architecture
	* Latency contributions from endpoints (client & server) (1pg)
		* Socket buffering & Offloads
		* Head of line blocking & retransmissions
		* Server resource contention
		* VMs/Containers 
* Metrics and methods for characterizing latency performance (5pgs)
	* Latency for a path in a live network is variable – a statistical distribution
		* Descriptive statistics: Average latency, min/mean/max/std
		* Order statistics (P0,P99,P99.9)
		* Packet Delay Variation vs. Inter-Packet Delay Variation vs. “Jitter”
		* CDFs
	* Measuring latency 
		* Protocols: ICMP (PING), UDP (STAMP, TWAMP, SamKnows, etc), TCP, HTTP/2 Echo
		* Awareness/manipulation of conditions (i.e., monitoring cross-traffic, introducing cross-traffic, etc.) 
			* Idle latency
			* Working latency
				* Latency Under Load
* How do latency and latency variation impact user experience? (incl. mention of latency mitigation techniques) (5pgs)
	* Mean Opinion Score 
	* VoIP, video conferencing
		* Impact to the call not feeling interactive 
		* User "talking over" other users 
		* Jitter buffers and how jitter increases microphone to speaker latency 
	* Multiplayer online gaming
		* Lag compensation techniques
		* Jitter buffers
	* Cloud gaming
		* Controller-to-photon latency 
	* Web browsing
		* Page Load Time & other metrics
