<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Inspecting Traffic Between Azure Virtual Machines</h1>
In this lab, I observe different kinds of network traffic to and from Azure virtual machines with Wireshark as well as experiment with network security groups. <br />



<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop Connection
- Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 Pro (21H2)
- Ubuntu Server 20.04

<h2> Setting-Up the Enviroment </h2>

I Configured two VMs in Azure within the same virtual network for communication. One VM runs Windows 10 Pro, and the other runs Ubuntu. I will be connecting to the Windows machine using RDP and SSH as a means of communicating with the Ubuntu machine within the virtual network.
<h2>Actions and Observations</h2>

<p>
<img src="https://i.imgur.com/DutP0XT.png" height="80%" width="80%" alt="Azure Networking Steps"/>
</p>
<p>
Using RDP, I connected to the Windows VM using its Public IP. Next, I installed Wireshark a well know program that is used to inspect network traffic. 
</p>
<br />

<p>
<img src="https://i.imgur.com/2JQawfN.png" height="80%" width="80%" alt="Azure Networking Steps"/>
</p>
<p>
Once Wireshark was downloaded and installed, I went ahead and filtered for ICMP traffic. Opened up Powershell and executed a Ping from VM1 (Windows) to VM2 (Ubuntu) using its Private IP within the Virtual network. Since Ping uses the ICPM protocol it is a very commonly use command when someone is trying to diagnose an issue with data transmission. Once I confirmed the Ping Request was successful, I initiated a constant ping with Ping -t (IP Address) to the Ubuntu VM in order to experiment with Network Security Groups within Azure.
</p>
<br />

<p>
<img src="https://imgur.com/a/p248R4f" height="80%" width="80%" alt="Azure Networking Steps"/>
</p>
<p>
Within the Azure portal, I created a new rule set-up to Deny ICMP with a with a pritority above 300 to make sure this rule would get applied before anything else. 
</p>
<br />

<p>
<img src="https://i.imgur.com/i3BC2LW.png" height="80%" width="80%" alt="Azure Networking Steps"/>
</p>
<p>
With the New Firewall rule in place I returned to the VM1's Windowed Connection through RDP. As the suspected I saw the Ping requests eventually time out. When I changed the rule back to Allow ICMP through the Firewall (NSG) the Ping Request reinitialized right back from where it started. 
</p>
<br />

<p>
<img src="https://i.imgur.com/fDtuLo9.png" height="80%" width="80%" alt="Azure Networking Steps"/>
</p>
<p>
Next, I wanted to check out the SSH traffic, so I logged into the Ubuntu server using PowerShell and the ssh command. Then, I used Wireshark to sift through the traffic on port 22.
</p>
<br />

<p>
<img src="https://i.imgur.com/mptFClI.png" height="80%" width="80%" alt="Azure Networking Steps"/>
</p>
<p>
After checking out the SSH traffic, I logged out of the Ubuntu server to dig into DHCP traffic. To see how it works, I tried grabbing a new IP address for my VM using the command "ipconfig /renew." This command momentarily disconnected me from the live RDP Session for a few seconds. Once I reconnected, I could see all the resulting traffic playing out in Wireshark.
</p>
<br />

<p>
<img src="https://i.imgur.com/gtiupfH.png" height="80%" width="80%" alt="Azure Networking Steps"/>
</p>
<p>
Continuing from the SSH Observation, I decided to analyze DNS traffic by employing the filter "udp.port == 53" along with the nslookup command. My intention was to observe the results of looking up two highly popular sites, google.com and disney.com. 
</p>
<br />

<p>
<img src="https://i.imgur.com/N7voXYU.png" height="80%" width="80%" alt="Azure Networking Steps"/>
</p>
<p>
To Conclude the lab, I chose to monitor RDP traffic with the filter "tcp.port == 3389." The traffic is continuous because RDP constantly streams live data from one computer to another. In my case, it involves my computer accessing a VM hosted on Azure, resulting in a constant flow of transmitted data.
</p>
<br />

<h2>Lessons Learned </h2>

In this lab, the primary objective is to gain insights into the dynamics of network communication among devices by examining the usage of different protocols and ports. While the lab may not involve direct troubleshooting, it plays a crucial role in information gathering. When actively troubleshooting network issues, I leverage essential tools such as Wireshark and the command line. These tools allow me to dissect and analyze the flow of traffic through various ports and protocols, providing a comprehensive understanding of the network's behavior.

Successful troubleshooting demands not only technical proficiency with these tools but also a curious and inquisitive mindset. Being familiar with the intricacies of network protocols and having a proactive approach to investigate and understand the traffic patterns are key elements for achieving success in resolving network issues. In essence, this lab serves as a valuable foundation, cultivating the skills and knowledge necessary for effective troubleshooting in real-world network scenarios.
