# README #

### What is this repository for? ###

sch_pi2: PI2 AQM (with dual queue option) module for Linux

iproute2-3.16.0: iproute2 package with added support for sch_pi2

#### Quick summary ####
PI2 AQM is both an extension, and a simplification of the PIE (PI-Enhanced AQM). It makes quite some heuristics unnecessary, while enabling it to control scalable (L4S = low loss, low latency and scalable) congestion controls (like DCTCP, TCP-Prague, ...). Both Reno/Cubic can be used in parallel with DCTCP, giving it the same throughput. 

The DualQ option can be used to classify the L4S traffic in a priority low latency queue, enabling L4S's full low latency power. The drop/mark coupling makes sure that the flows are still fair (now only congestion window fair as, due to the different Q sizes, their RTT will be different). 

### How do I get set up? ###

PI2 AQM is implemented as a module. In order to use it, you need to either reinstall iproute2 package or use a local build of tc from iproute2. The files that should be added/changed and a patch are included in this repository. Currently only tested with iproute2 version 3.16.0.

Build iproute2 locally:  
- download iproute2 package from https://www.kernel.org/pub/linux/utils/net/iproute2/ 
- apply patch or copy over the files manually

`cd iproute2-3.16.0 && make`

Reinstall iproute2:

`cd iproute2-3.16.0 && make install`

Build sch_pi2 module:  

`cd sch_pi2 && make`

Register module:  

`cd sch_pi2 && make load`

Remove module:  

`cd sch_pi2 && make unload`

Add pi2 as a qdisc with a bottleneck of 40Mbps:  

`sudo tc qdisc del dev <interface> root`

`sudo tc qdisc add dev <interface> root handle 1: htb default 10`

`sudo tc class add dev <interface> parent 1: classid 1:10 htb rate 40Mbit ceil 40Mbit burst 1516`

`sudo tc qdisc add dev <interface> parent 1:10 pi2` 


* Dependencies:

  bison libdb-dev flex


### Who do I talk to? ###

* Repo owner or admin
* Other community or team contact
