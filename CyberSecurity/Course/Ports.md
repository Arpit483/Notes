In the context of networking and cybersecurity, **ports** are communication endpoints used to identify specific processes or services on a host machine. They allow a device to distinguish between different services when multiple applications are running simultaneously.

### Types of Ports:
1. **Physical Ports**: These are hardware interfaces (e.g., USB ports, Ethernet ports) where you connect devices like keyboards, monitors, or network cables.

2. **Logical Ports (Network Ports)**: These are virtual ports that operate at the software level, facilitating network communication. They are identified by port numbers, ranging from 0 to 65535. Logical ports help devices differentiate between different types of network traffic.

### Port Numbers Breakdown:
1. **Well-known ports (0-1023)**: These are reserved for specific services and protocols like:
   - **Port 80**: HTTP (used for web traffic)
   - **Port 443**: HTTPS (secured web traffic)
   - **Port 22**: SSH (secure shell for remote administration)
   - **Port 25**: SMTP (used for sending emails)

2. **Registered ports (1024-49151)**: These are used for user-specific applications or services. Developers can register these ports with the IANA for custom applications.

3. **Dynamic or Private ports (49152-65535)**: These are used dynamically by applications to establish temporary connections. They're often used for client-side communications in network transactions.

### Commonly Used Ports:
- **Port 21**: FTP (File Transfer Protocol)
- **Port 53**: DNS (Domain Name System)
- **Port 110**: POP3 (Email retrieval protocol)
- **Port 3389**: RDP (Remote Desktop Protocol)

### How Ports Work:
When two devices communicate over a network (like a client and server), they use a combination of **IP addresses** and **ports** to route traffic correctly. The **IP address** specifies the destination, and the **port number** tells which application or service should handle the traffic on that destination device.

For example, if you're visiting a website, your computer sends a request to the server's IP address on **port 80** (for HTTP) or **port 443** (for HTTPS). The server responds to your computer's request, and the connection is made through these ports.

### Cybersecurity and Ports:
Ports are crucial in cybersecurity because attackers often scan them to find vulnerabilities. For instance:
- **Port scanning** is a technique used by attackers to identify open ports and services running on a system.
- **Firewall configuration** allows security teams to block or allow traffic based on port numbers.
- **Vulnerabilities on specific ports** can be exploited, like old versions of services running on certain ports.

### Securing Ports:
- **Close unused ports**: Minimize the attack surface by disabling services you don't need.
- **Use firewalls**: Block or limit access to sensitive ports.
- **Run services on non-default ports**: For example, instead of using port 22 for SSH, you can use an alternative port to evade basic port scanning attacks.

Do you want to dive deeper into any specific type of port or how they relate to cybersecurity?


[[OSI models]]

Types of Protocol
	https://www.geeksforgeeks.org/types-of-internet-protocols
	https://www.cloudflare.com/learning/network-layer/what-is-a-protocol/
