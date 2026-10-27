# Kali Linux: ARP Spoofing 

## Objective
Using Kali Linux and Windows 10 virtual machines, we will see my steps for coordinating an MITM attack by doing ARP poisoning to change the MAC address, allowing us to intercept the packets. Once done, we will see how someone can steal credentials from someone who is on an unprotected site.

##  What is ARP Spoofing?
<img width="700" height="380" alt="image" src="https://github.com/user-attachments/assets/11b052f6-36d6-404f-b7c6-0fbc6faf95ed" />

Before we explain what it is, we have to go over how two devices communicate in a network; let's think of a computer and a router. The computer will use Address Resolution Protocol (ARP) to connect the IP Address with the MAC Address (the physical Address of the device), and then store it in the ARP cache so it can remember which device has that MAC Address to send data to. 

So, when using a computer that is on a network, when you try to access something on the internet, it looks for the router by calling out its IP address; the router will
  call out back, saying that that's its IP and giving the MAC Address; then the computer stores the MAC Address and now knows where to communicate to, but let's say an attacker was on the network; then the attacker can be between both devices on the network by telling the victim's computer that it's the router, and that the attacker's MAC Address is the router's MAC Address, causing the victim to update the ARP cache and send data to the attacker, then forwards that data to the router; this is bad because if the data is not encrypted, the attacker can read the packets clearly.
### Tools Used
[Bullet Points - Remove this afterwards]

- Security Information and Event Management (SIEM) system for log ingestion and analysis.
- Network analysis tools (such as Wireshark) for capturing and examining network traffic.
- Telemetry generation tools to create realistic network traffic and attack scenarios.

## Steps
drag & drop screenshots here or use imgur and reference them using imgsrc

Every screenshot should have some text explaining what the screenshot is about.

Example below.

*Ref 1: Network Diagram*
