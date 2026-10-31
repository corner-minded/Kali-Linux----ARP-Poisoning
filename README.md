# Kali Linux: ARP Spoofing 

## Objective
Using Kali Linux and Windows 10 virtual machines, we will walk through the steps to coordinate an MITM attack using ARP poisoning to change the MAC address and intercept packets. Once done, we will see how someone can steal credentials from someone who is on an unprotected site.

### Tools Used

- Virtual Machine to have a safe environment to test the network attack.
- Free and open-source network security tool for man-in-the-middle attacks on a LAN.
- Packet sniffer to capture all encrypted and unencrypted traffic between the two devices.

##  What is ARP Spoofing?
<img width="700" height="380" alt="image" src="https://github.com/user-attachments/assets/11b052f6-36d6-404f-b7c6-0fbc6faf95ed" />

Before we explain what it is, we need to cover how two devices communicate on a network; let's think of a computer and a router. The computer will use Address Resolution Protocol (ARP) to connect the IP Address with the MAC Address (the physical Address of the device), and then store it in the ARP cache so it can remember which device has that MAC Address to send data to. 

So, when using a computer that is on a network, when you try to access something on the internet, it looks for the router by calling out its IP address; the router will
calls back, saying that's its IP and giving the MAC Address; then the computer stores the MAC Address and now knows where to communicate, but let's say an attacker is on the network.

Then the attacker can be between both devices on the network by telling the victim's computer that it's the router and that the attacker's MAC Address is the router's MAC Address, causing the victim to update the ARP cache and send data to the attacker, then forwarding that data to the router; this is bad because if the data is not encrypted, the attacker can read the packets clearly.

## Steps

### Setting up

``` sudo sysctl -w net.ipv4.ip_forward=1 ```

Before we start, we need to use this command so our Kali can be capable of forwarding packets that are meant for other destinations (other than itself)

<img width="1902" height="826" alt="image" src="https://github.com/user-attachments/assets/45e74c20-450c-4dbb-8b09-5e75de2890de" />
<img width="607" height="72" alt="image" src="https://github.com/user-attachments/assets/e6b114df-ac25-4fa3-b0e0-e7352a143c3a" />

```
Kali: 192.168.1.30 
Windows 10: 192.168.1.136
Router: 192.168.1.1
```
Here are the Kali and Windows IPs; we'll use the Windows and gateway (router) IPs to monitor their communications on the network. Now we will start setting up the attack using an open-source tool that comes with Kali.

### Ettercap

<img width="621" height="327" alt="image" src="https://github.com/user-attachments/assets/c9de42e8-b453-4e9e-b79c-392ce606d04c" />

Ettercap is mainly used to perform MITM attacks on a network; this will allow us to tell the Windows computer to change the router's MAC address to Kali's. First, we will set the netmask to show all available hosts on the network; we can see this above.

<img width="807" height="287" alt="image" src="https://github.com/user-attachments/assets/5e9ad0f1-8dfd-4227-a833-da73fdf64a2e" />

Now, looking at the host list, we can see both Windows and the router's IPs (sometimes I see only one or none of the IPs I'm looking for, so I just go to the current target menu and add them myself). Once you're here, you want to select the router's IPs, then send it to target 1; do the same for the Windows IPs, but for target 2, allowing us to focus between the to host on the network.

<img width="490" height="301" alt="image" src="https://github.com/user-attachments/assets/bf69fe5f-b02e-4463-9e5d-2cab2514df18" />

<img width="637" height="132" alt="image" src="https://github.com/user-attachments/assets/45e60f6f-49dd-478f-9270-d5839c302275" />

Finally, we can start ARP Spoofing.

<img width="747" height="141" alt="image" src="https://github.com/user-attachments/assets/6fd75c82-87c2-4e5e-9558-09c84a442777" />

We can see this by going into Windows 10 and running a command in Command Prompt to view all physical addresses. As shown above, the Kali and router's physical addresses are the same because Ettercap copied the router's physical address and told Windows 10 that the right place to forward the data is the Kali. Anyways, now we can start packet capturing.

<img width="1872" height="875" alt="image" src="https://github.com/user-attachments/assets/3f36cf43-a6b6-45a8-9ecf-8322bfd1647b" />

We are going to use a popular free-to-use application that can help us: Wireshark. It is used for computer network analysis, software, and communications protocol development. Using this, we will be able to steal website credentials from Windows 10.

<img width="1700" height="797" alt="image" src="https://github.com/user-attachments/assets/1832dd7f-b6a3-48ad-960e-4c8f1ac701e9" />

<img width="700" height="500" alt="image" src="https://github.com/user-attachments/assets/93eb8053-e864-4384-8ab4-7ddd7a886f31" />

We are going to use an unprotected website called Unicronitems.com. It claims to be a site where you can buy cute stuff and also claims to be affiliated with Amazon and is safe to buy here, even though it is using port 80 to run the site. There's no real proof behind the Amazon thing, so I just conclude it's a fake site.

Since we are capturing packets between the Windows machine and the router, I am going to put fake credentials in the account section on the site. Since this site works on port 80, any packet sent through this site will be unencrypted, which means anyone can read the packets in clear text.
