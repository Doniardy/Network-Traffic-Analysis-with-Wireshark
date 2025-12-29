# Network Traffic Analysis with Wireshark

## Project Overview
This project demonstrates basic network traffic analysis using Wireshark on a Kali Linux virtual machine.  
The analysis focuses on DNS and HTTP traffic generated during normal web browsing activities.

The goal of this project is to understand how web traffic appears at the packet level and how it can be analyzed for security monitoring purposes.

---

## Tools & Environment
- Kali Linux (Virtual Machine)
- Wireshark
- tcpdump
- Web Browser

---

## What Was Done
- Captured live network traffic using tcpdump
- Analyzed DNS queries and responses in Wireshark
- Inspected an HTTP session to observe credential and session handling
- Documented findings in a separate analysis report

Detailed technical analysis can be found in:
- `analysis/web_traffic_analysis.md`

---

## Key Takeaways
- DNS traffic can reveal visited domains and resolved IP addresses
- HTTP traffic transmits data in clear text and poses security risks
- Understanding normal traffic is important for alert monitoring and incident analysis

---

## Disclaimer
This project was completed in a controlled lab environment for educational purposes only.
