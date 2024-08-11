---
title: "Nmap: The Ultimate Network Scanning Tool"
description: "An in-depth guide on Nmap, its features, and why it's a preferred tool for network scanning and security auditing."
date: 2024-08-11
categories: [Tools, Nmap]
tags: [nmap, network-scanning, vulnerability-detection]
image:
  path: thbnls/nmap.png
---

### What is Nmap?

Nmap, short for Network Mapper, is an open-source Linux command-line tool used for network scanning. It allows users to scan IP addresses and ports in a network and detect installed applications. Network administrators use Nmap to identify devices on their network, discover open ports and services, and detect vulnerabilities.

Gordon Lyon, known by his pseudonym Fyodor, developed Nmap to help map entire networks easily and find open ports and services. Nmap has gained significant popularity and has even been featured in movies like The Matrix and the TV series Mr. Robot.

### Why Use Nmap?

There are several reasons why security professionals prefer Nmap over other scanning tools:

1. **Efficient Network Mapping**: Nmap quickly maps out a network with sophisticated commands or configurations, supporting both simple commands (e.g., checking if a host is up) and complex scripting through the Nmap scripting engine.

2. **Device Recognition**: Nmap can quickly identify all devices on a network, including servers, routers, switches, and mobile devices.

3. **Service Identification**: It helps identify services running on a system, such as web servers, DNS servers, and other common applications. Nmap can also detect application versions with reasonable accuracy to help identify existing vulnerabilities.

4. **Operating System Detection**: Nmap can find information about the operating system running on devices, providing detailed information like OS version. This makes it easier to plan additional approaches during penetration testing.

5. **Security Auditing and Vulnerability Scanning**: During security audits, Nmap can attack systems using existing scripts from the Nmap scripting engine.

6. **Graphical User Interface**: Nmap has a graphical user interface called Zenmap, which helps develop visual mappings of a network for usability and reporting.

Nmap's combination of speed, accuracy, and versatility makes it a preferred tool for network scanning and security auditing.

### What is a Port Scan?

A port scan is a technique used to identify open ports and services available on a networked device. Both hackers and cybersecurity professionals use this method to discover vulnerabilities and assess security measures.

##### Purpose and Usage:
- **Hackers**: Cybercriminals use port scanning to identify open ports, which can serve as entry points for unauthorized access or attacks. By analyzing the responses from these ports, they can determine if there are any potential weaknesses or exploitable services.
- **Businesses and IT Professionals**: Organizations use port scanning to identify vulnerabilities within their networks. By sending packets to specific ports and analyzing the responses, they can detect security issues and take steps to secure their systems.

##### Common Port Scanning Tools:
- **Nmap (Network Mapper)**: A popular tool used to discover hosts and services on a computer network.
- **Netcat**: A networking utility for reading from and writing to network connections using TCP or UDP.
- **IP Scanners**: Tools that scan IP addresses and identify open ports and services.

##### Information Provided by a Port Scan:
- **Services Running**: Identifies which services are active on the device.
- **Service Ownership**: Determines which users own the running services.
- **Anonymous Login**: Checks if anonymous logins are permitted.
- **Authentication Requirements**: Identifies which network services require authentication.

##### Benefits and Risks:
- **Benefits**: Helps organizations secure their networks by identifying potential vulnerabilities before they can be exploited by attackers.
- **Risks**: If performed by malicious actors, port scanning can lead to unauthorized access and exploitation of network vulnerabilities.

Port scanning is a double-edged sword, serving both as a valuable tool for cybersecurity professionals and a method for attackers to find weak points in a network. Proper use and understanding of this technique are crucial for maintaining network security.

### Port Scanning Techniques

**1. Ping Scans:**
   - **Description:** Ping scans, also known as Internet Control Message Protocol (ICMP) requests, are the simplest form of port scanning.
   - **How it Works:** It sends a series of ICMP requests to multiple servers, aiming to get a response.
   - **Use Case:** Administrators use ping scans to troubleshoot network issues.
   - **Detection:** Pings can be blocked or disabled by firewalls.

**2. Vanilla Scan:**
   - **Description:** A vanilla scan is a basic technique that attempts to connect to all 65,536 ports simultaneously.
   - **How it Works:** It sends a SYN (synchronize) flag, and if it receives a SYN-ACK (synchronize-acknowledge) response, it replies with an ACK (acknowledge) flag, completing the TCP handshake.
   - **Accuracy:** This method is accurate.
   - **Detection:** Easily detectable because the full TCP connection is logged by firewalls.

**3. SYN Scan:**
   - **Description:** Also known as a half-open scan, it is a stealthier method compared to a vanilla scan.
   - **How it Works:** It sends a SYN flag to the target and waits for a SYN-ACK response. If a SYN-ACK is received, the scanner does not respond back, leaving the TCP connection half-open.
   - **Speed:** This technique is quick.
   - **Detection:** Less detectable since the connection is not fully established, minimizing logging.

**4. XMAS and FIN Scans:**
   - **XMAS Scan:**
     - **Description:** Named for the set of flags that are turned on within a packet, appearing to blink like a Christmas tree in a protocol analyzer.
     - **How it Works:** Sends a packet with multiple flags set. The response can reveal information about the firewall and the state of the ports.
   - **FIN Scan:**
     - **Description:** Sends a packet with the FIN (finish) flag.
     - **How it Works:** Helps understand the level of activity and provides insight into the target’s firewall usage.
   - **Stealth:** Both methods are more discrete than basic scans.

**5. FTP Bounce Scan:**
   - **Description:** This technique disguises the sender's location by using an FTP server to relay packets.
   - **How it Works:** The attacker sends packets to the target via an FTP server, making it harder to trace the origin of the scan.

**6. Sweep Scan:**
   - **Description:** A preliminary port scanning technique used to identify active systems on a network.
   - **How it Works:** Sends traffic to a specific port across multiple computers on a network.
   - **Outcome:** Determines which systems are active but does not provide detailed port activity information.

These port scanning techniques are essential tools for both network administrators, for troubleshooting and securing networks, and attackers, for identifying potential vulnerabilities. Understanding these methods helps in enhancing network security by implementing appropriate defensive measures.

### The OSI Model

The Open Systems Interconnection (OSI) model is a conceptual framework used to describe the functions of a networking system. It divides the communication process into seven distinct layers. Introduced in 1983 and adopted as an international standard by ISO in 1984, the OSI model was the first standard model for network communications, embraced by major computer and telecommunication companies.

Although the modern Internet is primarily based on the simpler TCP/IP model, the OSI 7-layer model remains widely used. It helps visualize and communicate how networks operate and is instrumental in isolating and troubleshooting networking problems.

#### The Seven Layers of the OSI Model:

![Pasted image 20240729204131](https://github.com/user-attachments/assets/0553ad4d-f594-482e-aaed-916ccc45e2a8)


1. **Physical Layer:**
   - **Function:** Transmits raw bitstreams over a physical medium.
   - **Components:** Cables, switches, hubs, and other physical devices.
   - **Protocols and Technologies:** Ethernet, USB, Bluetooth.

2. **Data Link Layer:**
   - **Function:** Provides node-to-node data transfer and handles error correction from the physical layer.
   - **Components:** Network interface cards (NICs), switches.
   - **Protocols:** Ethernet, PPP (Point-to-Point Protocol), MAC (Media Access Control).

3. **Network Layer:**
   - **Function:** Manages device addressing, tracks the location of devices on the network, and determines the best way to move data.
   - **Components:** Routers.
   - **Protocols:** IP (Internet Protocol), ICMP (Internet Control Message Protocol).

4. **Transport Layer:**
   - **Function:** Provides reliable data transfer services to the upper layers, manages end-to-end communication, and controls flow and error correction.
   - **Components:** Gateways, firewalls.
   - **Protocols:** TCP (Transmission Control Protocol), UDP (User Datagram Protocol).

5. **Session Layer:**
   - **Function:** Manages sessions between applications. It establishes, maintains, and terminates connections between applications.
   - **Components:** Not typically represented by physical devices.
   - **Protocols:** NetBIOS, PPTP (Point-to-Point Tunneling Protocol).

6. **Presentation Layer:**
   - **Function:** Translates data between the application layer and the network format. It handles data encoding, encryption, and compression.
   - **Components:** Not typically represented by physical devices.
   - **Protocols:** SSL/TLS (Secure Sockets Layer/Transport Layer Security), JPEG, ASCII.

7. **Application Layer:**
   - **Function:** Provides network services directly to applications. It handles high-level APIs, including resource sharing, remote file access, and directory services.
   - **Components:** End-user applications.
   - **Protocols:** HTTP, FTP, SMTP, DNS.

The OSI model is essential for understanding and designing a robust network architecture. It enables interoperability between different products and software, and its layered approach allows for targeted troubleshooting and enhancements.

### Lab Setup 

In this guide, we'll walk through setting up the Mr. Robot VM from VulnHub and finding its IP address using Nmap.
####  **VM Import and Configuration**

**Step 1: Import the VM**

First, download the [Mr. Robot 1](https://download.vulnhub.com/mrrobot/mrRobot.ova) VM image to your Downloads folder. To import it into VirtualBox, use the following command:

```bash
VBoxManage import Downloads/mrRobot.ova
```

![[Pasted image 20240811095935.png]]

**Step 2: Adjust VM Settings**

After importing the VM, you need to adjust some settings to ensure proper functionality:

- **Display Settings:** Change the Graphic Controller from `VBoxVGA` to `VMSVGA`. This adjustment helps with better display compatibility and performance.

  ![[Pasted image 20240811100208.png]]

- **Network Adapter:** Set the network adapter to `Bridged Adapter`. This configuration allows the VM to be on the same network as the host machine, enabling full network interaction.

![[Pasted image 20240811100305.png]]

#### **Why Use a Bridged Adapter?**

Using a bridged adapter is crucial for creating a realistic network environment. Here’s why:

- **Same Network as Host:** The VM gets its own IP address on the same network as the host machine. This setup is useful when you want the VM to interact with other devices on the local network, such as for SSH, web servers, or other network-related tasks.

- **Direct Communication:** Other machines on the network can directly communicate with the VM. This is important for penetration testing or lab environments where simulating external attacks or accessing the VM’s services from other devices is necessary.

- **Avoiding NAT Limitations:** NAT (Network Address Translation) mode can restrict network interaction and requires port forwarding for external access. Bridged mode provides a unique IP address to the VM, avoiding these limitations.

- **Realistic Networking Environment:** Bridged mode ensures that the VM behaves like a real machine on the network, making lab scenarios and exercises more practical and applicable to real-world situations.

#### 3. **Finding the VM's IP Address**

Once the VM is booted and running, you need to find its IP address. You can use Nmap, a powerful network scanning tool, for this purpose.

**Step 1: Perform a Network Scan**

Run the following Nmap command to scan your local network and discover live hosts:

```bash
nmap -sn <network-address>/24
```

Replace `<network-address>/24` with your network’s address. For instance, if your local network is `192.168.1.0/24`, the command would be:

```bash
nmap -sn 192.168.1.0/24
```

**Example Output:**

```bash
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-08-11 10:10 IST
Nmap scan report for linux.bbrouter (192.168.1.4)
Host is up (0.00032s latency).
MAC Address: 08:00:27:17:18:CC (Oracle VirtualBox virtual NIC)
```

In this example, the IP address of the Mr. Robot VM is `192.168.1.4`.

#### Network Scanning with Nmap

Once you have the IP address, you can perform a detailed scan on the VM. Using the default Nmap scan to check for open ports, run:

```bash
nmap 192.168.1.4
```

**Output:**

```bash
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-08-11 10:18 IST
Nmap scan report for linux.bbrouter (192.168.1.4)
Host is up (0.00049s latency).
Not shown: 997 filtered tcp ports (no-response)
PORT    STATE  SERVICE
22/tcp  closed ssh
80/tcp  open   http
443/tcp open   https
MAC Address: 08:00:27:17:18:CC (Oracle VirtualBox virtual NIC)

Nmap done: 1 IP address (1 host up) scanned in 4.83 seconds
```

In this scan, the ports `80/tcp` (HTTP) and `443/tcp` (HTTPS) are open, while `22/tcp` (SSH) is closed.

Lets See The Background Process What Was Going On The Backend

For That We Can Use Wireshark

### What is wireshark ?

Wireshark is a powerful tool for capturing and analyzing network traffic. It helps in understanding what is happening on a network by providing a detailed view of packets being transmitted.

### **Wireshark Overview**

- **Purpose:** Wireshark allows you to capture and examine the data traveling over your network. It helps in diagnosing network issues, analyzing protocols, and understanding network behavior.

- **How It Works:** It captures packets from network interfaces and displays them in a user-friendly format. You can then analyze these packets to see the details of network communications.

### **Example Use Case:**

If you've scanned your network with Nmap and found open ports on a VM, you can use Wireshark to further investigate what is happening on those ports. For instance:

- **HTTP Traffic Analysis:** Start a Wireshark capture and filter for HTTP traffic (`tcp port 80`). This will show you the details of web requests and responses, helping you understand what data is being transmitted over the HTTP service.

- **Protocol Analysis:** Analyze specific protocols in use and see how they communicate. This can help in debugging issues or understanding potential vulnerabilities.

Wireshark is an invaluable tool for network administrators, security professionals, and anyone needing in-depth network analysis. It provides a granular view of network traffic and helps in diagnosing and understanding network-related issues.

## **Analyzing the Network Layer with Wireshark**

When we perform network analysis, understanding how data flows through the OSI (Open Systems Interconnection) model's layers is crucial. Wireshark provides the capability to capture and analyze packets at various layers, giving us insight into what is happening on the network. 

### **Identifying the Network Interface**

First, we need to determine which network interface is active and relevant to our analysis. We can use the `ifconfig` command to list all network interfaces and their configurations. In your case, the active interface is `wlan0`, which is a wireless network interface. Here's what the `ifconfig` output shows:

```bash
┌──(root㉿neo)-[~]
└─# ifconfig
lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 4977  bytes 250084 (244.2 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 4977  bytes 250084 (244.2 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

wlan0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.1.8  netmask 255.255.255.0  broadcast 192.168.1.255
        inet6 fe80::659d:1749:2c8b:417  prefixlen 64  scopeid 0x20<link>
        ether 50:c2:e8:43:93:3b  txqueuelen 1000  (Ethernet)
        RX packets 1905919  bytes 2321521463 (2.1 GiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 281162  bytes 46011062 (43.8 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

The output indicates that `wlan0` is the active interface with IP `192.168.1.8`. This is the interface we'll focus on for our Wireshark analysis.

![[Pasted image 20240811115406.png]]

### **Choosing the Network Interface in Wireshark**

Once you know your active interface, you can start Wireshark and select `wlan0` as the interface to monitor. This is done from the interface selection screen in Wireshark:
1. Select `wlan0` from the list of interfaces.

![[Pasted image 20240811115805.png]]

![[Pasted image 20240811120135.png]]

2. Start capturing packets by clicking on the start button.
### **Performing Nmap Scan and Monitoring with Wireshark**

Now, with Wireshark capturing traffic on `wlan0`, you can run an Nmap scan on your target IP. This scan will generate network traffic that Wireshark will capture. As the scan progresses, Wireshark will display packets associated with the Nmap scanning process, showing the interaction between your system and the target.

![[Pasted image 20240811121055.png]]
### **Analyzing the Scanned Traffic**

The Nmap scan interacts with the target using the OSI model layers, primarily focusing on the network and transport layers.

- **OSI Model Layers:**
  - **Layer 1 (Physical):** Deals with the hardware transmission of raw data over physical media.
  - **Layer 2 (Data Link):** Manages communication between adjacent network nodes and deals with MAC addresses.
  - **Layer 3 (Network):** Handles routing and forwarding, using IP addresses.
  - **Layer 4 (Transport):** Manages end-to-end communication, often using TCP or UDP.
  - **Layer 7 (Application):** Where protocols like HTTP, FTP, and DNS operate.

In Wireshark, you'll see the packets traversing these layers:

- **Ethernet (Layer 2):** Frames that include MAC addresses for source and destination.
- **IP (Layer 3):** Contains IP addresses and handles the routing of packets.
- **TCP/UDP (Layer 4):** Manages the establishment of connections and the transfer of data.
- **Application Layer (Layer 7):** Where protocols like HTTP or HTTPS operate.

### **Example Wireshark Capture Analysis**

1. **Ethernet Frame:**
   - Source MAC: Your device’s MAC address.
   - Destination MAC: Target's MAC address.

2. **IP Packet:**
   - Source IP: `192.168.1.8` (your machine).
   - Destination IP: Target IP (e.g., `192.168.1.4`).

3. **TCP/UDP Segment:**
   - Source Port: Random port chosen by your machine.
   - Destination Port: The port you're scanning (e.g., 80, 443).

4. **Application Data:**
   - Actual data exchanged (e.g., HTTP requests/responses if scanning an HTTP service).

Wireshark will show each layer in a hierarchical structure, allowing you to drill down into the specifics of the network interaction.

### **Interpreting the Data**

By analyzing the captured data, you can:

- **Identify Open Ports:** Confirm what services are running on the target machine.
- **Understand Network Behavior:** See how your machine interacts with the network during the scan.
- **Spot Anomalies:** Look for unusual patterns or unexpected responses that could indicate security issues.

