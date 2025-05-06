Reference books
![](https://i.imgur.com/N7FQ8cY.jpeg)

![](https://i.imgur.com/eT1SOkS.png)
## **Quick Recap**

- **IPv6 Address Length**: 128-bit address, significantly longer than IPv4 (32-bit).
- **Address Notation**: Represented in hexadecimal format, divided into 8 groups separated by colons (`:`).
- **Primary Address Types**:
    1. **Unicast** – Identifies a single interface.
    2. **Multicast** – Identifies multiple interfaces (one-to-many communication).
    3. **Anycast** – Identifies the nearest instance of a service.
- **No Broadcast in IPv6**: Broadcast is replaced with Multicast to improve efficiency.
- **IPv6 Features**:
    - Larger address space.
    - Improved security with IPsec.
    - Stateless Address Autoconfiguration (SLAAC).
    - Better Quality of Service (QoS) handling.

![](https://i.imgur.com/7KRA2i3.png)
## **Rules for Compressed Notation of IPv6**

IPv6 addresses can be **compressed** using two main rules:

### **Rule 1: Leading Zero Compression**

- **Leading zeros in each hextet can be omitted**.
- Example:
    - **Original**: `2001:0db8:0000:0000:0000:ff00:0042:8329`
    - **Compressed**: `2001:db8:0:0:0:ff00:42:8329`

### **Rule 2: Consecutive Zeros Compression**

- A **series of consecutive `0000` groups can be replaced with `::`** (double colon).
- This compression **can be used only once per address**.
- Example:
    - **Original**: `2001:0db8:0000:0000:0000:ff00:0042:8329`
    - **Compressed**: `2001:db8::ff00:42:8329`

### **Example of Incorrect Usage**

❌ `2001::db8::1` (invalid because `::` appears twice)

### **Expanding a Compressed Address**

- **Example**: `2001:db8::1`
- **Expanded**: `2001:0db8:0000:0000:0000:0000:0000:0001`
![](https://i.imgur.com/3XEyEC1.png)

![](https://i.imgur.com/zmqzC42.png)
## **Recap: Global Unicast and Link-Local Address**

### **Global Unicast Address (GUA)**

- Equivalent to **public IPv4 addresses**.
- Used for communication **over the internet**.
- Uniquely assigned and globally routable.
- **Prefix:** `2000::/3`

### **Link-Local Address (LLA)**

- **Automatically assigned** to every IPv6-enabled interface.
- Used for **local communication** (within the same link).
- **Prefix:** `FE80::/10`
- **Must not be routed** beyond the local link.
![](https://i.imgur.com/F0ycjag.png)

## **Focusing on the Global Unicast Address (GUA)**

- **Used for communication across different networks.**
- Assigned by **Internet Assigned Numbers Authority (IANA)** and distributed through ISPs.
- Has a structure that follows the **/64 prefix allocation** model.
- Ensures unique addressing on a **global scale**.
![](https://i.imgur.com/BONL2rk.png)
## **Why Prefer IPv6 Over IPv4?**

1. **IPv4 Exhaustion**
    
    - IPv4 has a **limited** number of addresses (approx. 4.3 billion).
    - IPv6 provides **340 undecillion addresses** (practically unlimited).
2. **Better Security**
    
    - **IPsec is built-in** for encryption and authentication.
    - More robust protection against attacks like **spoofing**.
3. **Efficient Routing**
    
    - Simplified packet forwarding due to hierarchical addressing.
4. **No NAT Required**
    
    - IPv6 eliminates the need for **Network Address Translation (NAT)**, allowing direct end-to-end connectivity.
5. **Enhanced Mobility & IoT Support**
    
    - More efficient address allocation and seamless mobile device operation.
![](https://i.imgur.com/zf06Hhc.png)
## **/64 Global Unicast Address and the 3-1-4 Rule**

IPv6 Global Unicast Addresses follow a structured format:
### **The /64 Prefix**

- **Standard for most IPv6 subnets**.
- The first **64 bits** represent the **network portion**, and the last **64 bits** identify the host.
### **3-1-4 Rule**

IPv6 addressing is structured as follows:

|**Section**|**Bit Length**|**Purpose**|
|---|---|---|
|**Global Routing Prefix**|48 bits|Assigned by ISPs for unique addressing.|
|**Subnet ID**|16 bits|Allows for internal subnetting within an organization.|
|**Interface ID**|64 bits|Unique identifier for an individual device.|

**Example Address Breakdown:**

- **2001:db8:** → Global Prefix (provided by ISP).
- **abcd:** → Subnet Identifier.
- **1234:5678:9abc:def0:** → Interface ID (host).

![](https://i.imgur.com/8KsYU4H.png)
#### **Why Use the 3-1-4 Rule?**

- **Simplifies IPv6 Addressing and Management.**
- **Enhances Network Scalability**, allowing organizations to create multiple subnets.
- **Supports Stateless Address Autoconfiguration (SLAAC)**, where devices can generate their own addresses using the Interface ID.
- **Ensures Global Uniqueness** and hierarchical addressing for efficient routing.
![](https://i.imgur.com/7pI5kDV.png)
### **Subnet Allocation and the /48 Rule in IPv6**

#### **1. Understanding IPv6 Subnet Allocation**

- In IPv6, **Internet Service Providers (ISPs)** allocate addresses using a **/48 prefix** to organizations.
- This **/48 allocation** allows organizations to create multiple subnets internally.

---
#### **2. How Many /64 Subnets in a /48 Allocation?**

- Each IPv6 **/64 subnet** consists of **16 bits for subnetting** within a **/48 allocation**.
    
- Since **16 bits are available for subnetting**, the total number of **/64 subnets** in a **/48 allocation** is:
    
    216=65,536 subnets2^{16} = 65,536 \text{ subnets}216=65,536 subnets
- This means that an organization receiving a **/48 block** can create up to **65,536 separate /64 subnets**.
    
---
#### **3. Why is /64 Used as a Standard Subnet Size?**

- **Supports Stateless Address Autoconfiguration (SLAAC).**
- **Provides a large number of host addresses per subnet** ( 2642^{64}264 possible addresses).
- **Maintains consistency across networks**, simplifying routing and management.
---
#### **4. Practical Allocation of IPv6 Subnets**

|**Prefix Size**|**Who Gets It?**|**Number of /64 Subnets**|
|---|---|---|
|**/32**|Large ISPs|65,536 **/48 subnets**|
|**/48**|Assigned to organizations|65,536 **/64 subnets**|
|**/64**|Used within an organization|1 subnet (for a single network)|
![](https://i.imgur.com/9jszACn.png)
### **IPv6 Enable Command**
#### **1. Purpose of the `ipv6 enable` Command**

- The `ipv6 enable` command is used to **activate IPv6 on an interface**.
- It **automatically assigns a link-local address** even if no global unicast address is configured.
---
#### **2. Configuration Steps**
```plaintext
Router(config)# interface fastethernet 0/1  
Router(config-if)# ipv6 enable  
Router(config-if)# end  
Router# show ipv6 interface brief  
```

- The command `ipv6 enable` is applied to **FastEthernet 0/1**.
- The `show ipv6 interface brief` command displays IPv6 addresses assigned to the interface.
- A **link-local address (FE80::/10)** is automatically generated.
---
#### **3. Key Points About Link-Local Addresses**

- **Automatically created** when a global unicast address is configured.
- Used for **local communication within a network segment** (e.g., router-to-router communication).
- They always start with **FE80::/10** and are unique per interface.
---
#### **4. Effects of the `ipv6 enable` Command**
- **Creates a link-local address** even if no global unicast address is assigned.
- **Maintains the link-local address** even when the global unicast address is removed.
This command is crucial for enabling IPv6 functionality on interfaces, allowing for essential network communication.
![](https://i.imgur.com/60eLB2L.png)
### **Pinging a Link-Local Address in IPv6**

#### **1. Understanding Link-Local Addresses**

- **Link-local addresses** are automatically assigned to every IPv6-enabled interface.
- These addresses are only valid within the same network segment (link) and always start with **FE80::/10**.
- Used for **local communication** such as router-to-router protocols (e.g., OSPFv3, EIGRPv6, and neighbor discovery).

---
#### **2. Key Requirements for Pinging a Link-Local Address**

- **Exit Interface Must Be Specified**:
    - When using `ping` with a **link-local address**, the **exit interface** must be explicitly mentioned.
    - This is because **link-local addresses are identical across multiple links**, and the router needs to know which interface to use.

---
#### **3. Example of Pinging a Link-Local Address**

##### **Incorrect Command:**

```plaintext
R1# ping fe80::2
Output Interface: ser 0/0/0
% Invalid interface. Use full interface name without spaces (e.g. Serial0/1)
```

- The command fails because **the exit interface is not fully specified**.
##### **Correct Command:**

```plaintext
R1# ping fe80::2%serial0/0/0
```

- By specifying **serial0/0/0**, the router knows which interface to send the ICMP request through.
- The command successfully sends **5 ICMP echo requests**, indicated by `!!!!!`.

---
#### **4. Key Takeaways**

- **Link-local addresses must always be used with an interface identifier** in commands like `ping` and `traceroute`.
- If not specified, the router does not know which interface to use, leading to errors.
- Always use the **full interface name** when prompted (e.g., `Serial0/0/0`).
- Link-local addresses are essential for **router-to-router communication** and network discovery.
![](https://i.imgur.com/lm8vr8U.png)
