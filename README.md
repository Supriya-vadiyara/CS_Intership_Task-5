# Task 5: Network Traffic Analysis with Wireshark

This report documents the process of capturing and analyzing live network traffic using Wireshark. The goal was to identify and inspect several common internet protocols to understand their function.

---

### 1. Objective

To capture live network packets from my machine, filter them to isolate specific protocols, and analyze their function to better understand how network communication works at both the local and internet level.

---

### 2. Methodology

1.  **Installation:** Installed `Wireshark` and its `Npcap` packet capture driver on my local machine.
2.  **Capture:** Started a live packet capture on my active `Wi-Fi` interface.
3.  **Traffic Generation:** Generated network traffic by simply using my computer normally for a few minutes, which included background application traffic and web browsing.
4.  **Analysis:** Stopped the capture and used Wireshark's display filters to isolate and inspect individual protocols.
5.  **Export:** Saved the complete traffic capture as a `.pcapng` file.

---

### 3. Analysis of Identified Protocols

The capture contained a wide variety of traffic. I successfully identified and analyzed the following three key protocols:

#### Protocol 1: ARP (Address Resolution Protocol)
*   **Filter Used:** `arp`
*   **Purpose:** ARP is a fundamental protocol for **local networks**. Its job is to figure out the hardware MAC address of a device when you only know its local IP address. This is necessary for devices on the same Wi-Fi or Ethernet network to communicate directly.
*   **Observation:** The capture clearly showed ARP in action. I observed **ARP Request** packets where my computer was asking for the MAC address of my router (`Who has 192.168.0.105?`), followed by **ARP Reply** packets where the router responded with its unique hardware address.

#### Protocol 2: DNS (Domain Name System)
*   **Filter Used:** `dns`
*   **Purpose:** DNS acts as the internet's phonebook. It translates human-friendly domain names (like `www.google.com`) into computer-friendly IP addresses.
*   **Observation:** I saw a packet from my computer sending a "Standard query" to a DNS server. A few moments later, a response packet came back from the server providing the IP address, allowing my computer to then connect to the website.

#### Protocol 3: XMPP (Extensible Messaging and Presence Protocol)
*   **Filter Used:** `xmpp`
*   **Purpose:** XMPP is a protocol used for instant messaging (chat) and presence (seeing if contacts are online/offline). It is used by many gaming clients and chat applications.
*   **Observation:** The capture showed several XMPP packets being sent in the background. I was able to identify `IQ (Info/Query)` packets, which are used for managing the connection, and `Presence` packets, which update online status. This indicates that a chat application was running on my PC during the capture.

#### **Protocol 3: TCP (Transmission Control Protocol)**
*   **Filter Used:** `tcp`
*   **Purpose:** TCP is the workhorse protocol for **reliable connections**. It ensures that all data sent between a client and a server arrives in the correct order and without errors. It is used for nearly all web browsing, email, and file transfers.
*   **Observation:** The capture clearly showed the famous **TCP three-way handshake** used to start a connection to Google's server:
    1.  `[SYN]` - My PC sends a "synchronize" request.
    2.  `[SYN, ACK]` - Google's server sends back a "synchronize-acknowledge."
    3.  `[ACK]` - My PC sends a final "acknowledge," and the connection is established.

#### **Protocol 4: TLS (Transport Layer Security)**
*   **Filter Used:** `tls`
*   **Purpose:** TLS is the protocol that provides **encryption** for our online communication. It is the "S" in `HTTPS`. Its job is to create a secure, encrypted tunnel *after* a TCP connection has been established, ensuring that no one can read the data being exchanged.
*   **Observation:** Immediately following the TCP handshake, I saw a series of TLS packets. The exchange began with a `Client Hello` from my PC, followed by a `Server Hello` from Google. This "TLS Handshake" is where my browser and the server securely agree on encryption keys. After the handshake, all subsequent data was labeled `Application Data`, which is the encrypted HTTP traffic that Wireshark cannot read.

---

### 4. Conclusion

This task was a practical introduction to network traffic analysis. Using Wireshark, I was able to visualize the "invisible" conversations that happen every time I use the internet. Filtering for specific protocols like DNS, TCP, TLS, ARP and XMPP made it clear how these different systems work together to deliver a simple webpage.
