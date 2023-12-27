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

I Configured two VMs in Azure within the same virtual network for communication. One VM runs Windows 10 Pro, and the other runs Ubuntu. I will be connecting to the Windows machine using RDP as a means of communicating with the Ubuntu machine within the virtual network.
<h2>Actions and Observations</h2>

<p>
<img src="https://i.imgur.com/DutP0XT.png" height="80%" width="80%" alt="Azure Networking Steps"/>
</p>
<p>
Using RDP, I connected to the Windows VM using its Public IP. Next, I installed Wireshark a well know program that is used to inspect network traffic across the network. 
</p>
<br />

<p>
<img src="https://i.imgur.com/2JQawfN.png" height="80%" width="80%" alt="Azure Networking Steps"/>
</p>
<p>
Once Wireshark was downloaded and installed, I went ahead and filtered for ICMP traffic. Opened up Powershell and executed a Ping from VM1 (Windows) to VM2 (Ubuntu) using its Private IP within the Virtual network. Since Ping uses the ICPM protocol it is a very commononly use command when someone is trying to diagnose an issue with data transmission. Once I confirmed the Ping Request was successful, I initiated a constant ping with Ping -t (IP Address) to the Ubuntu VM in order to experiment with Network Security Groups within Azure.
</p>
<br />

<p>
<img src="https://i.imgur.com/HTga2Iq.png" height="80%" width="80%" alt="Azure Networking Steps"/>
</p>
<p>
Within the Azure portal, I created a new rule set-up to Deny ICMP with a with a pritority above 300 to make sure this rule would get applied before anything else. 
</p>
<br />

<p>
<img src="https://i.imgur.com/i3BC2LW.png" height="80%" width="80%" alt="Azure Networking Steps"/>
</p>
<p>
With the New Firewall rule in place I returned to the VM1's Windowed Connection through RDP. As the suspected I saw the Ping requests eventually time out. When I changed the rule back to Allow ICMP through the Firewall (NSG) the Ping Request reinitialized right back from where they started. 
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
After examining SSH traffic, I exited the Ubuntu server in order to filter for DHCP traffic. To see it in action, I decided to attempt to issue a new IP address from my VM. The command ipconfig /renew will attempt to issue the new IP address and will temporarily disconnect me for a few seconds. After reconnecting, the resulting traffic is shown in Wireshark.
</p>
<br />

<p>
<img src="https://i.imgur.com/gtiupfH.png" height="80%" width="80%" alt="Azure Networking Steps"/>
</p>
<p>
To observe DNS traffic, I used the filter udp.port == 53 and the command nslookup. I wanted to see the results that are from looking up google.com and disney.com, two very popular sites. 
</p>
<br />

<p>
<img src="https://i.imgur.com/N7voXYU.png" height="80%" width="80%" alt="Azure Networking Steps"/>
</p>
<p>
To finish my lab, I decided to observe RDP traffic. The filter for Wireshark is tcp.port == 3389. There is non-stop traffic because RDP is constantly showing me a live stream from one computer to another (in my case, my computer accessing the VM that is hosted on Azure) and thus traffic is always transmitted. 
</p>
<br />

<h2>Lessons Learned </h2>

The purpose of this lab is for me to see how different protocols and ports are utilized in a network between devices. While this lab does not exactly allow me to troubleshoot, it still serves a purpose to gather information. While troubleshooting, I need to utilize different tools like Wireshark and the command line to see how traffic flows in a network through ports and protocols. Familiarity and an inquisitive mind are key to success!
