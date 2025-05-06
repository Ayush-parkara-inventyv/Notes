#### **1. Routing Protocols & IPv6 Compatibility**
- **Routing Protocols Readiness**
    - Both **Exterior Gateway Protocols (EGPs)** and **Interior Gateway Protocols (IGPs)** support IPv6.
    - **BGP (Border Gateway Protocol):** Uses **Multiprotocol Extensions** to carry IPv6 prefix information.
    - **OSPFv3 (Open Shortest Path First Version 3):** Supports IPv6 within a domain.
    - **EIGRP for IPv6 (Enhanced Interior Gateway Routing Protocol):** IPv6-compatible version of EIGRP.
    - **RIPng (Routing Information Protocol Next Generation):** IPv6 version of RIP, still used despite expectations of its retirement.
#### **2. Enabling IPv6 on Cisco Routers**
- Command: `ipv6 unicast-routing` (entered in global configuration mode).
- Ensures the router is **ready to run IPv6 routing protocols** (if supported by the router's code).
- Use **Cisco Feature Navigator** to verify IPv6 feature support on a router.
#### **3. RIPng (RIP Next Generation) Features**
- **Surprisingly still exists** in the IPv6 environment.
- **Similarities to IPv4 RIP:**
    - 15-hop limit.
    - Split Horizon and Poison Reverse.
    - Hop count as a metric.
- Very few differences between **RIP for IPv4** and **RIPng for IPv6**.
#### **4. General Similarities Between IPv4 & IPv6 Routing Protocols**
- Most routing protocols **retain their core functionalities** in the transition to IPv6.
- Example: **OSPFv3 is very similar to OSPFv2** with minor changes.
- If a network professional has **mastered IPv4 routing**, transitioning to IPv6 will be relatively easy.
#### **5. IPv6 Transition Strategies**
- The discussion ends with a question on how to **easily transition into an IPv6 world**, implying the need for transition mechanisms.

![](https://i.imgur.com/y8EkxZQ.png)

by this command we can make the device run on the **eigrp** and **bgp** for v6
```undefined
ipv6 unicast-routing
```
![](https://i.imgur.com/3OfbeRs.png)

just make ensure that your router supports these ipv6 protocols to make this work and to use this we have to use the navigator feature in the cisco's website
**Router Internet Protocol**
![](https://i.imgur.com/1B6fXT9.png)

so for the transitioning in to the ipv6 world, we can do that easily by this way called **dual stack** (unable to hear clearly so can check) 

![](https://i.imgur.com/1LK23Mn.png)
by this way we can run v4 and v6 at the same time using the double stack.

we can also say that transitioning from ipv4 to v6 can by this way can be said to be done by tunneling between the protocols
![](https://i.imgur.com/oiXvZt9.png)
e.g.
by the help of dual stack we can connect v6 network at the headquaters and to the v6 network branch 
we can connect them using the dual stack routers and v4 network traffic through the tunnelling and by this way we can connect the protocols and all using this methodology and can do the transition easily

Better version to understand this is the picture below
![](https://i.imgur.com/0tTaA0C.png)

so no need to shutdown the network for transition to v6 from the v4
also we can use both of them together easily .