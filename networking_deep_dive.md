# Deep Dive: Networking Fundamentals for AWS Solutions Architect (SAA-C03)
*A comprehensive guide based on the core architectural principles of global networking.*

---

## 1. The OSI 7-Layer Model: The Foundation
Understanding the OSI model is critical because AWS services are often categorized by the layer they operate on (e.g., WAF at Layer 7, NLB at Layer 4).

### The Layers
1.  **Layer 7 - Application:** Where the user interacts (HTTP, HTTPS, DNS, SSH). This is where **Application Load Balancers (ALB)** and **WAF** live.
2.  **Layer 6 - Presentation:** Handles data formatting, encryption, and decryption (SSL/TLS).
3.  **Layer 5 - Session:** Manages sessions between applications.
4.  **Layer 4 - Transport:** Manages host-to-host communication. 
    * **TCP:** Connection-oriented, reliable, guaranteed delivery.
    * **UDP:** Connectionless, fast, "best-effort" (Voice/Video).
    * **Network Load Balancers (NLB)** operate here.
5.  **Layer 3 - Network:** Logical addressing and routing.
    * **IP Addresses:** How devices find each other globally.
    * **Routers:** Devices that move packets between different networks.
6.  **Layer 2 - Data Link:** Physical hardware addressing.
    * **MAC Address:** The unique ID burned into every network card.
    * **Switches:** Move "Frames" within a single local network.
7.  **Layer 1 - Physical:** The actual electrical signals, fiber optics, or radio waves.

---

## 2. IPv4, Subnetting & CIDR
In AWS VPCs, you define network boundaries using **Classless Inter-Domain Routing (CIDR)** notation.

### The Math of a Subnet
An IPv4 address is 32 bits ($4 	ext{ octets} 	imes 8 	ext{ bits}$).
* **/32:** Represents a single IP address (all bits fixed).
* **/24:** Represents 256 addresses ($2^{8}$). The first 24 bits are the network.
* **/16:** Represents 65,536 addresses ($2^{16}$).

### AWS Reserved IP Addresses
In every AWS subnet, **5 IP addresses** are reserved and cannot be used:
1.  **x.x.x.0:** Network Address.
2.  **x.x.x.1:** VPC Router.
3.  **x.x.x.2:** Amazon Provided DNS (Amazon Route 53 Resolver).
4.  **x.x.x.3:** Future use.
5.  **x.x.x.255:** Network Broadcast Address (AWS does not support broadcast, but it is still reserved).

---

## 3. The TCP Three-Way Handshake
To establish a "reliable" connection, TCP performs a handshake. This is why TCP has higher latency than UDP.
1.  **SYN:** Client sends a synchronization request.
2.  **SYN-ACK:** Server acknowledges and sends its own sync request.
3.  **ACK:** Client acknowledges the server.
*Connection is now ESTABLISHED.*

---

## 4. NAT: Gateway vs. Instance (Exam Favorite)
You must understand how private subnets reach the internet.

| Feature | NAT Gateway (AWS Managed) | NAT Instance (Self-Managed) |
| :--- | :--- | :--- |
| **Scaling** | Automatic up to 45 Gbps. | Manual; limited by EC2 instance size. |
| **High Availability** | Managed by AWS; highly available within an AZ. | You must manage failover yourself. |
| **Security Groups** | Cannot be associated with a security group. | Associated with a security group. |
| **Maintenance** | None (AWS patches it). | You must patch the OS. |
| **Port Forwarding** | Not supported. | Can be configured manually. |

---

## 5. DNS (Domain Name System)
DNS is the "Phonebook of the Internet," converting names (google.com) to IPs (142.250.x.x).
* **A Record:** Maps Name $
ightarrow$ IPv4.
* **AAAA Record:** Maps Name $
ightarrow$ IPv6.
* **CNAME:** Maps Name $
ightarrow$ Name (e.g., `www` to `lb-1234.aws.com`). Cannot be used for the "naked" domain (example.com).
* **Alias Record (AWS Specific):** Maps a name to an AWS Resource (ALB, S3, CloudFront). Unlike CNAME, it **can** be used for the naked domain.

---

## 6. Key Networking Terminology
* **TTL (Time to Live):** How long a DNS record is cached.
* **MTU (Maximum Transmission Unit):** The largest frame size (Standard is 1500 bytes; AWS Jumbo Frames are 9001 bytes).
* **Latency:** The delay in data travel (measured in ms).
* **Throughput:** The amount of data that can be moved in a given time (Gbps).
