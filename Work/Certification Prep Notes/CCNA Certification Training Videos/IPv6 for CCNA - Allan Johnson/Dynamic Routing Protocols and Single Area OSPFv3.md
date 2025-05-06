### **What are Dynamic Routing Protocols?**

Dynamic Routing Protocols are used in networking to automatically determine the best paths for data to travel across a network. Unlike static routing, where routes are manually configured, dynamic routing adjusts to network changes, such as link failures or congestion.
### **Types of Dynamic Routing Protocols**
There are mainly three types of dynamic routing protocols:
1. **Distance Vector Protocols** – Determine the best path based on hop count (e.g., RIP - Routing Information Protocol).
2. **Link-State Protocols** – Maintain a complete topology of the network to determine the best route (e.g., OSPF - Open Shortest Path First).
3. **Hybrid Protocols** – Combine aspects of both distance vector and link-state protocols (e.g., EIGRP - Enhanced Interior Gateway Routing Protocol).
![](https://i.imgur.com/GEPIJ0S.png)
### **1. Routing Protocol Classification**
Routing protocols are divided into **Interior Gateway Protocols (IGPs)** and **Exterior Gateway Protocols (EGPs)**:
#### **A. Interior Gateway Protocols (IGPs)**
IGPs are used within a single autonomous system (AS). They are further categorized into:
1. **Distance Vector Routing Protocols**
    
    - Uses hop count as the metric for determining the best path.
    - Periodically shares routing updates with neighbors.
    - Common protocols:
        - **IPv4:** RIP v2 (Routing Information Protocol version 2)
        - **IPv6:** RIPng (RIP next generation)
2. **Link-State Routing Protocols**
    
    - Each router has a complete map of the network and updates occur when there are topology changes.
    - Common protocols:
        - **IPv4:** OSPFv2 (Open Shortest Path First version 2), IS-IS (Intermediate System to Intermediate System)
        - **IPv6:** OSPFv3, IS-IS for IPv6
3. **Hybrid Routing Protocols**
    
    - Combines features of both Distance Vector and Link-State protocols.
    - **EIGRP (Enhanced Interior Gateway Routing Protocol)** belongs to this category:
        - **IPv4:** EIGRP
        - **IPv6:** EIGRP for IPv6
#### **B. Exterior Gateway Protocols (EGPs)**
EGPs operate between autonomous systems, commonly used for interconnecting different organizations’ networks.

- **Path Vector Protocol:**
    - **IPv4:** BGP-4 (Border Gateway Protocol version 4)
    - **IPv6:** BGP-4 for IPv6
---
### **2. IPv4 vs. IPv6 Routing Protocols**
The table in the image shows the equivalent routing protocols in IPv4 and IPv6:
- Many IPv6 protocols are simply updated versions of their IPv4 counterparts.
- The main difference is that in configuration, "ip" commands in IPv4 are replaced with "ipv6" for IPv6.
Example:
- In IPv4: `router ospf 1`
- In IPv6: `router ospfv3 1`
---
### **3. Upcoming IPv6 Routing Protocol - RFC 6550**

The note at the bottom mentions **RFC 6550**, which refers to:

- **RPL (Routing Protocol for Low-Power and Lossy Networks)**, sometimes called “Ripple.”
- It is specifically designed for **low-power and lossy networks (LLNs)** such as IoT (Internet of Things) devices.
---
### **Summary**
- **Interior Gateway Protocols (IGPs)** handle routing within a single autonomous system.
- **Exterior Gateway Protocols (EGPs)**, like **BGP**, manage routing between autonomous systems.
- IPv6 routing protocols mostly mirror their IPv4 counterparts with some improvements.
- **RFC 6550 (RPL)** is an emerging protocol designed for IoT and low-power networks.
![](https://i.imgur.com/xYBjBgW.png)

## **What is EIGRP for IPv6?**
EIGRP (Enhanced Interior Gateway Routing Protocol) is a **hybrid routing protocol** that combines features of both distance-vector and link-state protocols. It was initially developed by Cisco for IPv4 but has been adapted for IPv6 with some differences.
### **Key Features of EIGRP for IPv6:**
1. **Supports IPv6 Addressing**
    
    - Uses IPv6 link-local addresses for neighbor discovery and communication.
    - Does not require a network-wide IPv6 address configuration.
2. **Uses the Diffusing Update Algorithm (DUAL)**
    
    - Ensures fast convergence and loop-free routing.
    - Allows routers to quickly adapt to topology changes.
3. **Independent Configuration from IPv4**
    
    - Unlike OSPF, which has OSPFv2 (IPv4) and OSPFv3 (IPv6), EIGRP for IPv6 is configured separately from EIGRP for IPv4.
4. **No Need for a Network Command**
    
    - Unlike EIGRP for IPv4, EIGRP for IPv6 does not use the `network` command to enable routing on interfaces.
    - Instead, interfaces must be explicitly enabled using `ipv6 eigrp AS_number`.
5. **Uses Reliable Transport Protocol (RTP)**
    
    - Ensures reliable delivery of routing updates.
6. **Automatic Route Summarization Disabled**
    
    - Unlike IPv4 EIGRP, auto-summary is **disabled by default** in IPv6.
![](https://i.imgur.com/aQzHKxl.png)

### **Key Elements in the Diagram:**
1. **Two Routers (R1 and R2):**
    
    - These routers are configured to run EIGRP for both **IPv4** and **IPv6**.
    - Each router maintains separate routing information for IPv4 and IPv6.
2. **Separate IPv4 and IPv6 Networks:**
    
    - The **upper half** of the diagram represents an **IPv4 network**, showing EIGRP tables for IPv4.
    - The **lower half** represents an **IPv6 network**, showing EIGRP tables for IPv6.
    - Both IPv4 and IPv6 networks function independently but follow the same EIGRP principles.
3. **EIGRP for IPv4 Tables (Gray Boxes, Upper Section):**
    
    - **Neighbor Table:** Stores information about directly connected EIGRP neighbors.
    - **Topology Table:** Contains all learned routes and their associated metrics.
    - **Routing Table:** Stores the best available routes used for forwarding packets.
4. **EIGRP for IPv6 Tables (Blue Boxes, Lower Section):**
    
    - Similar to IPv4, EIGRP for IPv6 maintains:
        - **Neighbor Table**
        - **Topology Table**
        - **Routing Table**
    - The **main difference** is that EIGRP for IPv6 uses IPv6 addresses and operates independently from IPv4.
---
### **Key Differences Between EIGRP for IPv4 and IPv6:**

|Feature|EIGRP for IPv4|EIGRP for IPv6|
|---|---|---|
|Addressing|Uses IPv4 addresses|Uses IPv6 addresses (link-local)|
|Auto-Summary|Enabled by default|Disabled by default|
|Neighbor Discovery|Uses IPv4 addresses|Uses IPv6 link-local addresses|
|Configuration|Uses `network` command|Configured per interface (no `network` command)|

---
### **Important Observations:**

1. **EIGRP for IPv4 and EIGRP for IPv6 operate separately** even though they run on the same physical routers.
2. **Routing tables are separate** for IPv4 and IPv6, meaning:
    - IPv4 routes cannot be used for IPv6 traffic.
    - IPv6 routes are managed independently from IPv4.
3. **EIGRP for IPv6 uses link-local addresses** for neighbor discovery, whereas IPv4 EIGRP uses normal IPv4 addresses.
---
### **Summary:**

- **EIGRP for IPv4 and IPv6 function similarly** but are managed separately.
- **IPv6 uses different addressing mechanisms** (link-local addresses).
- **Routing tables, topology tables, and neighbor tables are separate** for IPv4 and IPv6.
- **No "network" command for EIGRP IPv6**; instead, interfaces must be explicitly enabled.
![](https://i.imgur.com/iMGo6FC.png)

### **Key Differences Between EIGRP for IPv4 and IPv6:**
1. **Addressing:**
    
    - IPv4 EIGRP uses standard IPv4 addresses.
    - IPv6 EIGRP uses **IPv6 link-local addresses** for neighbor communication.
2. **Multicast Destination Address:**
    
    - **EIGRP for IPv4** uses **224.0.0.10** for multicast updates.
    - **EIGRP for IPv6** uses **FF02::10** for multicast updates.
3. **Configuration:**
    
    - **EIGRP for IPv4** uses the `network` command to enable interfaces.
    - **EIGRP for IPv6** does not use `network` commands. Instead, it is enabled directly on interfaces.
4. **Authentication:**
    
    - **IPv4 EIGRP** supports both plain-text and MD5 authentication.
    - **IPv6 EIGRP** only supports MD5 authentication for security reasons.
5. **Router ID:**
    
    - Both IPv4 and IPv6 use a **32-bit Router ID**, typically based on the highest IPv4 address or manually assigned.
![](https://i.imgur.com/5sVsSWZ.png)

### **Key Points:**
- **EIGRP for IPv6** maintains three tables:
    1. **Neighbor Table** (Tracks directly connected routers)
    2. **Topology Table** (Contains all learned routes)
    3. **Routing Table** (Stores best paths to destinations)
- **Source Address:** IPv6 **link-local address**
    - EIGRP for IPv6 uses **link-local addresses** (FE80::/10) for neighbor communication.
- **Destination Address:**
    - Either **FF02::10** (EIGRP IPv6 multicast)
    - Or another **IPv6 link-local address** (for direct neighbor communication).
### **Why Link-Local Addresses?**
- These addresses are **unique per link** and required for neighbor discovery.
- **No need for global IPv6 addresses** for EIGRP to function.
![](https://i.imgur.com/wUwhYdO.png)

### **Key Components:**
1. **Routers:**
    
    - **R1, R2, and R3** are interconnected using **Serial (S0/0/0, S0/0/1, etc.)** and **Gigabit (G0/0)** interfaces.
2. **IPv6 Global Unicast Addresses:**
    
    - Each router has been assigned a **global IPv6 network** for each link.
    - Example:
        - **R1 ↔ R2** uses **2001:DB8:CAFE:A001::/64**
        - **R2 ↔ R3** uses **2001:DB8:CAFE:A002::/64**
        - **R1 ↔ R3** uses **2001:DB8:CAFE:A003::/64**
3. **IPv6 Link-Local Addresses (FE80::/10):**
    
    - Used for **neighbor discovery and EIGRP communication**.
    - Each router has a unique **FE80::x** address:
        - **R1:** FE80::1
        - **R2:** FE80::2
        - **R3:** FE80::3
4. **Internet Connectivity:**
    
    - **R2 connects to the ISP** using **2001:DB8:FEED:1::/64**.
### **Why Link-Local Addresses?**

- **Link-local addresses (FE80::/10) are required** for EIGRP for IPv6 because:
    - They allow direct communication between neighbors.
    - They **do not require global IPv6 addresses** to form adjacencies.
### **Conclusion:**
This is a well-configured **IPv6 EIGRP topology**, where:
- Routers use **global unicast addresses** for routing.
- **Link-local addresses** facilitate EIGRP neighbor relationships.
- **R2 provides external connectivity to the Internet**. 🚀
![](https://i.imgur.com/XiaSPc6.png)


### **Configuring EIGRP for IPv6 Routing Process:**
This image shows the step-by-step process to configure **EIGRP (Enhanced Interior Gateway Routing Protocol) for IPv6** on a router.
### **Configuration Steps:**

1. **Enable EIGRP for IPv6:**
    
    ```
    R1(config)# ipv6 router eigrp 2
    ```
    
    - However, the error `% IPv6 routing not enabled` appears because IPv6 routing is not yet activated.
2. **Enable IPv6 Routing Globally:**
    
    ```
    R1(config)# ipv6 unicast-routing
    ```
    
    - This command **enables IPv6 routing** on the router.
3. **Re-enter EIGRP Configuration Mode:**
    
    ```
    R1(config)# ipv6 router eigrp 2
    ```
    
    - The number `2` is the **EIGRP autonomous system (AS) number**, which must be the same on all routers in the network.
4. **Set the EIGRP Router ID:**
    
    ```
    R1(config-rtr)# eigrp router-id 1.0.0.0
    ```
    
    - **EIGRP requires a 32-bit router ID**, which is similar to an IPv4 address.
    - This ID helps identify the router in the network.
5. **Activate EIGRP for IPv6:**
    
    ```
    R1(config-rtr)# no shutdown
    ```
    
    - This command **enables** the EIGRP process (by default, it's in a shutdown state).
### **Key Takeaways:**
- **EIGRP uses a 32-bit Router ID** for both IPv4 and IPv6.
- If the router has **no IPv4 interfaces**, the `eigrp router-id` **must be manually set**.
- **The Router ID must be unique** to prevent routing conflicts.
### **Final Thought:**
This configuration **activates and enables EIGRP for IPv6** on the router, ensuring it can participate in IPv6 routing. 🚀
![](https://i.imgur.com/t71iJFZ.png)


(change the order first image and then content for upper portion from below it follows this format)
![](https://i.imgur.com/fIrPYzm.png)
### **Enabling EIGRP for IPv6 on the Interface**
This image shows how to enable **EIGRP for IPv6** on specific interfaces.
### **Key Differences from IPv4 Configuration:**
- **No "network" command is needed** (unlike IPv4 EIGRP).
- Instead of defining networks, EIGRP for IPv6 is **enabled per interface**.
### **Configuration Breakdown:**
1. **Enter Interface Configuration Mode:**
    
    ```
    R1(config)# interface g0/0
    ```
    
    - This selects the **GigabitEthernet0/0** interface.
2. **Enable EIGRP on the Interface:**
    
    ```
    R1(config-if)# ipv6 eigrp 2
    ```
    
    - This enables **EIGRP for IPv6 with AS number 2** on that interface.
3. **Repeat for Serial Interfaces:**
    
    ```
    R1(config-if)# interface s0/0/0
    R1(config-if)# ipv6 eigrp 2
    ```
    
    ```
    R1(config-if)# interface s0/0/1
    R1(config-if)# ipv6 eigrp 2
    ```
    
    - These commands enable EIGRP for **serial interfaces** (S0/0/0 and S0/0/1).
### **Key Takeaways:**
✅ **No "network" command is needed** like in IPv4.  
✅ EIGRP is **enabled directly on each interface**.  
✅ This method ensures **efficient routing updates only on specific interfaces**.
This setup allows the router to **participate in IPv6 EIGRP routing** on selected interfaces 🚀.

![](https://i.imgur.com/hQUzkCo.png)
### **Performing the Same Process on Other Routers in the Domain**
This image shows the **configuration of EIGRP for IPv6** on a second router (**R2**) in the network. The process follows the same steps as the previous router (**R1**), ensuring that EIGRP is enabled and adjacency is formed.
### **Configuration Breakdown:**
1. **Enable IPv6 Routing:**
    
    ```
    R2(config)# ipv6 unicast-routing
    ```
    
    - This **enables IPv6 routing** on the router.
2. **Enable EIGRP for IPv6:**
    
    ```
    R2(config)# ipv6 router eigrp 2
    ```
    
    - This **creates an EIGRP for IPv6 instance** with **AS (Autonomous System) number 2**.
3. **Set a Unique Router ID:**
    
    ```
    R2(config-rtr)# eigrp router-id 2.0.0.0
    ```
    
    - Since **EIGRP for IPv6 still requires a 32-bit Router ID** (similar to IPv4), this sets a unique ID for **R2**.
4. **Activate EIGRP Process:**
    
    ```
    R2(config-rtr)# no shutdown
    ```
    
    - This **activates the EIGRP for IPv6 process**.
5. **Enable EIGRP on Interfaces:**
    
    ```
    R2(config)# interface g0/0
    R2(config-if)# ipv6 eigrp 2
    R2(config-if)# exit
    ```
    
    ```
    R2(config)# interface s0/0/0
    R2(config-if)# ipv6 eigrp 2
    R2(config-if)# exit
    ```
    
    ```
    R2(config)# interface s0/0/1
    R2(config-if)# ipv6 eigrp 2
    ```
    
    - Instead of using the **network** command (which is used in IPv4), EIGRP is enabled **per interface**.
### **Neighbor Adjacency Formation:**
```
%DUAL-5-NBRCHANGE: EIGRP-IPv6 2: Neighbor FE80::1 (Serial0/0/0) is up: new adjacency
```
- This message indicates that **R2 has successfully formed an EIGRP neighbor adjacency** with another router (**R1** in this case).
- The neighbor's **IPv6 link-local address (FE80::1)** is shown.
- The adjacency was formed on **interface Serial0/0/0**.


![](https://i.imgur.com/ja6pont.png)
### **Verifying EIGRP for IPv6 Neighbor Adjacencies**
This image shows the **verification command** used to check EIGRP for IPv6 neighbor adjacencies on **Router R1**.

---
### **Command Used:**

```
R1# show ipv6 eigrp neighbors
```

This command displays the **EIGRP IPv6 neighbors** that have formed an adjacency with **R1**.

---
### **Output Breakdown:**

|H|Address (Link-Local)|Interface|Hold (sec)|Uptime|SRTT (ms)|RTO|Q Cnt|Seq Num|
|---|---|---|---|---|---|---|---|---|
|1|FE80::3|Serial 0/0/1|13|00:37:17|45|270|0|8|
|0|FE80::2|Serial 0/0/0|14|00:53:16|32|2370|0|8|

- **H (Handle)** → Internal number used by the router to track the neighbor.
- **Link-local Address** → IPv6 link-local addresses of the neighbors (**FE80::3** and **FE80::2**).
- **Interface** → The router interface where adjacency was formed (**Serial 0/0/1** and **Serial 0/0/0**).
- **Hold (sec)** → Time left before declaring the neighbor down (timer resets when hello packets are received).
- **Uptime** → How long the adjacency has been established.
- **SRTT (ms) (Smooth Round Trip Time)** → Time taken for an EIGRP packet to reach the neighbor and return.
- **RTO (Retransmission Timeout)** → How long the router waits before retransmitting unacknowledged packets.
- **Q Count** → Number of EIGRP packets in the queue (should be **0** in a stable network).
- **Seq Num (Sequence Number)** → The last sequence number received from the neighbor.
---
### **Key Observations:**

✅ **Two neighbors are formed** with link-local addresses **FE80::2** and **FE80::3** on different serial interfaces.  
✅ **EIGRP for IPv6 uses link-local addresses** for neighbor discovery and communication (instead of global IPv6 addresses).  
✅ **Uptime indicates stability** – both adjacencies have been up for a significant time.  
✅ **SRTT and RTO values are reasonable**, indicating good connectivity.  
✅ **Q Count is 0**, meaning no packets are stuck waiting to be sent.

---
### **Conclusion:**
This verification confirms that **EIGRP for IPv6 is successfully running**, and Router R1 has **formed neighbor adjacencies** with two other routers. 🎯

![](https://i.imgur.com/tKprulU.png)
### **Verifying EIGRP for IPv6 Learned Prefixes**
This image shows the output of the **`show ipv6 route eigrp`** command on **Router R1**, which displays the IPv6 routes learned via **EIGRP**.

---
### **Command Used:**

```
R1# show ipv6 route eigrp
```

This command lists all **IPv6 routes** that R1 has learned through **EIGRP**.

---
### **Output Breakdown:**

|Code|Network Prefix|Administrative Distance / Metric|Next Hop (Link-Local)|Interface|
|---|---|---|---|---|
|D|2001:DB8:CAFE:2::/64|[90/3524096]|FE80::3|Serial0/0/1|
|D|2001:DB8:CAFE:3::/64|[90/2170112]|FE80::3|Serial0/0/1|
|D|2001:DB8:CAFE:A002::/64|[90/3523840]|FE80::3|Serial0/0/1|

- **D (EIGRP learned route)** → These routes were dynamically learned via **EIGRP**.
- **2001:DB8:CAFE:X::/64** → These are IPv6 prefixes that R1 has learned.
- **[90/xxxxx]** → The first value (90) represents the **Administrative Distance (AD)** of EIGRP, and the second value is the **Feasible Distance (FD)**, which represents the total metric to reach the destination.
- **via FE80::3** → The next-hop address to reach these networks is **FE80::3**, which is a **link-local IPv6 address**.
- **Serial 0/0/1** → The interface on which the next-hop neighbor is located.
---
### **Key Observations:**
✅ **Routes are learned dynamically via EIGRP**, as indicated by the **D** code.  
✅ **Link-local addresses (FE80::3) are used as next-hop addresses**, which is standard for **EIGRP over IPv6**.  
✅ **All learned routes are accessible via Serial0/0/1**, meaning they are being received from the same neighbor.  
✅ **Different networks (subnets) are being advertised into EIGRP**, ensuring proper routing in the IPv6 network.


![](https://i.imgur.com/ylVjjES.jpeg)
### **EIGRP for IPv6 Manual Summarization**
This image illustrates the concept of **manual route summarization in EIGRP for IPv6**.

---
### **Key Points in the Diagram:**
- **Router R3** is connected to multiple **Loopback Networks** with the following IPv6 prefixes:
    - `2001:DB8:ACAD:1::/64`
    - `2001:DB8:ACAD:2::/64`
    - `2001:DB8:ACAD:3::/64`
    - `2001:DB8:ACAD:4::/64`
- Instead of advertising each `/64` prefix separately, R3 **manually summarizes** them into a **single /48 prefix**:
    - **`2001:DB8:ACAD::/48`**
- This summarized route is then **advertised out of Serial interfaces (S0/0/0 and S0/0/1)**.
- The summary route **includes all /64 prefixes** within the range:
    - `2001:DB8:ACAD:0000::/64` to `2001:DB8:ACAD:FFFF::/64`
---
### **Why is Manual Summarization Used?**

✅ **Reduces the number of routing entries** → Instead of advertising four separate `/64` networks, only one `/48` route is advertised.  
✅ **Optimizes routing tables** → Less memory usage and faster lookups.  
✅ **Improves network efficiency** → Reduces unnecessary EIGRP updates and decreases network congestion.  
✅ **Enhances route scalability** → New subnets within the summarized range (`2001:DB8:ACAD:X::/64`) will be automatically included in the summary route.

---
### **Conclusion:**

Manual summarization in **EIGRP for IPv6** helps simplify the routing process by aggregating multiple smaller networks into a single summary route. In this case, all `/64` networks under `2001:DB8:ACAD::/48` are efficiently advertised as one route. 🚀

![](https://i.imgur.com/Qy3Z3ml.jpeg)



![](https://i.imgur.com/yFkSS5r.png)

![](https://i.imgur.com/FZYrenk.png)

![](https://i.imgur.com/9mZapJE.png)

![](https://i.imgur.com/m2JabOj.png)

![](https://i.imgur.com/GqLlzyr.png)

![](https://i.imgur.com/8g235L3.png)

![](https://i.imgur.com/kXmjI8h.png)

![](https://i.imgur.com/4eaULlO.png)

![](https://i.imgur.com/Oh2kxja.png)

![](https://i.imgur.com/dg9M7wZ.png)

![](https://i.imgur.com/1kMEGJL.png)

![](https://i.imgur.com/zTAwqQS.png)

![](https://i.imgur.com/Jyeul6M.png)

![](https://i.imgur.com/4KwCO8X.png)

![](https://i.imgur.com/fPdomNb.png)

![](https://i.imgur.com/ShJXFVb.png)
