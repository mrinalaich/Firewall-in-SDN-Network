# Firewall based SDN Network
Staring with Firewall based SDN

## Introduction
In SDN Networks, the network intelligence is logically centralized in software-based controllers (the control plane), and network devices become simple packet forwarding devices (the data plane) that can be programmed via an open interface (OpenFlow).

## Firewall
SDN could be used to replace expensive Layer 4-7 firewalls, load-balancers, and IPS/IDS with cost-effective high-performance switches with a logically centralized controller.
In this project, I have implemented Layer 3 & 4 firewall by proactive policies and managed to block several applications like host-to-host connectivity and destination process.

---
## Architecture
![alt tag](https://github.com/MrinalAich/SDN/blob/master/Architecture.png)
---
## Flow Diagram
![alt tag](https://github.com/MrinalAich/SDN/blob/master/Flow_Diagram.png)
---

### Running the tests
There are two scripts namely, mininetScript.py and poxController_firewall.py which will run the mininet based ovsSwitch and POX Controller respectively.

* Running the POX Controller  
 * Go to the pox directory installed in your setup and execute
```
./pox.py log.level --DEBUG poxController_firewall.py
```

* Running the Mininet and OVS-Switch  
 * Execute the python script  
```
python mininetScript.py
```
  
* Running sample tests  
 * Layer-3 Host-to-Host Connectivity blocked  
   ```
   h1 ping h4
   ```
  * Output: The packets will be dropped by the Controller.

 * Layer-4 Process blocked  
  * Run two IPerf servers on Host-h3  
    ```
    h3 iperf -s -p 80 &
    h3 iperf -s -p 22 &
    ```
  * Connect to both the servers  
    ```
    h2 iperf –c h3 –p 80 –t 2 –i 1
    h2 iperf –c h3 –p 22 –t 2 –i 1
    ```
  * Output: All traffic to Destination h3 and port 80 are dropped by the Controller.

## Authors
* **Mrinal Aich** - [Link](http://cse.iith.ac.in/)
