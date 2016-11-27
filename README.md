# Firewall based SDN Network
Staring with Firewall based SDN

## Introduction
In SDN Networks, the network intelligence is logically centralized in software-based controllers (the control plane), and network devices become simple packet forwarding devices (the data plane) that can be programmed via an open interface (OpenFlow).

## Firewall
SDN could be used to replace expensive Layer 4-7 firewalls, load-balancers, and IPS/IDS with cost-effective high-performance switches with a logically centralized controller.
In this project, I have implemented Layer 2, 3 & 4 firewall by proactive policies and managed to block several applications like specific link, host-to-host connectivity and destination process.

## Architecture
There are one each POX Controller and OVS-Switch alongwith 4 Hosts forming an SDN Network.
![alt tag](https://github.com/MrinalAich/SDN/blob/master/Architecture.png)

---

## Flow Diagram  
The flowchart shows how Rule check is done by the SDN controller on receiving a OFPT_PACKET_IN message from OVS-Switch.
![alt tag](https://github.com/MrinalAich/SDN/blob/master/FlowDiagram.png)

---

## Prerequisites  
Your system should install [mininet](http://mininet.org/download/) and [pox](https://openflow.stanford.edu/display/ONL/POX+Wiki#POXWiki-InstallingPOX) packages.

## Installing
There are two scripts namely, mininetScript.py and poxController_firewall.py which will run the mininet based ovsSwitch and POX Controller respectively.

* Running the POX Controller  
 * Go to the pox directory installed in your setup and execute  
   ```
   ./pox.py log.level --DEBUG poxController_firewall
   ```

* Running the Mininet and OVS-Switch  
 * Execute the python script  
   ```
   python mininetScript.py
   ```

## Firewall Rules installed in the controller
* Layer-2 Link connection blocked  
  Block all traffic from Host-h2  
  ```
  AddRule(dataPath_id, 00:00:00:00:00:02, NULL, NULL, NULL)
  ```
* Layer-3 Host-to-Host Connectivity blocked  
  Block all traffic from Host-h1 to Host-h4  
  ```
  AddRule(dataPath_id, NULL, 10.0.0.1, 10.0.0.4, NULL)
  ```
* Layer-4 Destination Process blocked  
  Block all traffic destined to Process h3 at port 80  
  ```
  AddRule(dataPath_id, NULL, NULL, 10.0.0.3, 80)
  ```

## Running the Tests
* Layer-2 Link connection blocked  
  ```
  h2 ping h4
  ```
 * Output: The ARP L2-packets will be dropped by the Controller.

* Layer-3 Host-to-Host Connectivity blocked  
  ```
  h1 ping h4
  ```
 * Output: The IP Layer-packets will be dropped by the Controller.

* Layer-4 Destination Process blocked  
 * Run two IPerf servers on Host-h3  
   ```
   h3 iperf -s -p 80 &
   
   h3 iperf -s -p 22 &
   ```
 * Connect to both the servers  
   ```
   h1 iperf –c h3 –p 80 –t 2 –i 1
   
   h1 iperf –c h3 –p 22 –t 2 –i 1
   ```
 * Output: All TCP-traffic to Destination h3 and port 80 are dropped by the Controller.

---

## Authors
* **Mrinal Aich** - [Link](http://cse.iith.ac.in/sites/default/files/Images/mtech%202016/CS16MTECH11009.pdf)

## Acknowledgments
* **Prof. Kotaro Kataoka** - [Link](http://cse.iith.ac.in/profile/faculty/kotaro/)
