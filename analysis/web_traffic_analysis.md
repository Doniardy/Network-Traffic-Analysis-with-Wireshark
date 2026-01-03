# Network Sniffing with Wireshark

## Overview
This project documents my hands-on practice in capturing and analyzing network traffic using tcpdump and Wireshark on Kali Linux.
The traffic captured comes from normal web browsing activities, specifically when accessing **google.com** and **netacad.com**.

The lab focuses on understanding how DNS resolution and HTTP sessions work by observing real packet captures.

## Objectives
- Capture live network traffic from a Kali Linux machine
- Save captured packets into a PCAP file
- Analyze DNS traffic and HTTP sessions  using Wireshark
- Understand how activity  appears at the packet  level

## Environment
- Operating System: Kali Linux (Virtual Machine)
- Network Interface: eth0
- Tools Used:
	- tcpdump
	- Wireshark
- Websites accessed during capture:
	- google.com
	- netacad.com
	- 10.6.6.13

## Streps Performed

### 1. Network Preparation
The Kali Linux virtual machine was startedd and basic network information was verified. This included checking the IP address, MAC address, default gateway, and DNS server to ensure the system was properly connected to the network.

### 2. Packet Capture
Network traffic was captured using tcpdump with the following command:


```bash
sudo tcpdump -i eth0 -s 0 -w /home/kali/Documents/network-traffic-analysis-wireshark/pcap/packetdump.tcap

```
While tcpdump was running, a web browser was used to access:

- google.com
- netacad.com

After completing the browsing activity, the capture process was stopped and the packets were saved into a PCAP file.

### 3. Packet Analysis (DNS and HTTP Session)

After completing the packet capture, the PCAP file was analyzed using Wireshark to observe DNS activity and an HTTP session in clear text.

#### DNS Traffic Analysis
The capture file was opened in Wireshark and filtered using the `dns` display filter.  
DNS queries and responses were observed when accessing google.com and netacad.com. In addition to the main domains, multiple DNS lookups were generated for external resources referenced by the webpages.

By inspecting the DNS Standard Query packet for `netacad.com`, it was observed that:
- The source MAC address belonged to the Kali Linux VM
- The destination MAC address was the default gateway, since the DNS server was on a different Layer 2 network
- The DNS response returned one or more IP addresses associated with the `netacad.com` domain

This demonstrates how DNS traffic can reveal which websites a user is visiting and which IP addresses are resolved for those domains.

#### HTTP Session Analysis
An HTTP session was analyzed by capturing traffic between the Kali Linux client and a local DVWA web server running in a Docker container.

Wireshark was configured to capture traffic on the interface connected to the `10.6.6.0/24` network. A browser was used to access the DVWA login page via its IP address. Since the web server uses HTTP instead of HTTPS, all communication was transmitted in clear text.

By searching for `POST` packets, the login request was identified. Inspection of the HTTP POST packet showed that:
- Form data contained the submitted username and password
- Credentials were visible in plain text within the packet payload

Further analysis of HTTP responses showed a `302 Found` message containing a `Set-Cookie` header. The server issued a session cookie (`PHPSESSID`) to the client.  
The subsequent HTTP GET request from the client included the same session ID in the Cookie header, confirming that the session identifier was reused to maintain the authenticated session.

This analysis highlights the security risks of unencrypted HTTP communication, including credential exposure and the potential for session hijacking.

### 4. Security Relevance

This analysis demonstrates how network traffic inspection can be used to support security monitoring activities.  
DNS analysis helps identify which domains are accessed by users, which is useful for detecting suspicious or malicious domains.

The HTTP session analysis shows how sensitive information such as credentials and session cookies can be exposed when traffic is not encrypted. This highlights the importance of HTTPS and secure session handling.

Understanding normal DNS and web traffic behavior is essential for:
- Creating alert baselines
- Identifying abnormal traffic patterns
- Supporting incident investigation and alert triage

### 5. Limitations

This analysis was performed in a controlled lab environment with limited traffic sources.  
The captured data represents normal user activity and does not include malicious behavior.

Encrypted HTTPS traffic was not decrypted, therefore payload inspection was limited to unencrypted HTTP sessions only.

### 6. Conclusion

This project provided hands-on experience in capturing and analyzing network traffic using Wireshark.  
Through DNS and HTTP analysis, normal web communication patterns were identified and security risks related to unencrypted traffic were demonstrated.

The skills practiced in this project are directly applicable to entry-level security monitoring and alert analysis roles, where understanding baseline network behavior is critical for detecting anomalies and potential threats.
