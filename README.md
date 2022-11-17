<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, DNS, DHCP, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Create Resources in Microsoft Azure (Virtual Machines/Compute)
- Initiate and Observe ICMP Traffic
- Initiate and Observe SSH Traffic
- Initiate and Observe DHCP Traffic
- Initiate and Observe DNS Traffic

<h2>Actions and Observations</h2>

<p>
For this tutorial, there will be two virtual machines utilized. 
The first virtual machine (VM1) will run Windows 10 as its base operating system. 
The second virtual machine (VM2) will have a base operating system of Ubuntu Server 20.04. 
Both virtual machines will be under the same resource group and virtual network (Vnet).
This can be confirmed by viewing the topology available under Network Watcher in Azure.
</p>
<p>
<img src="https://i.imgur.com/7yqJC3J.png" height="70%" width="70%" alt="Network Watcher"/>
</p>
<br />

<p>
Now use Remote Desktop Connection to connect to the Windows 10 virtual machine and install Wireshark within your Windows 10 virtual machine.
</p>
<p>
<img src="https://i.imgur.com/iCSRzPk.png" height="70%" width="70%" alt="Wireshark Install"/>
</p>
<br />

<p>
Once Wireshark is installed, open the application, and begin capturing packets.
</p>
<p>
<img src="https://i.imgur.com/FjsxFsw.png" height="70%" width="70%" alt="Start Capturing Packets"/>
</p>
<br />

<p>
Then, only filter ICMP traffic in Wireshark.
</p>
<p>
<img src="https://i.imgur.com/0G2YRZb.png" height="70%" width="70%" alt="Filter ICMP Traffic"/>
</p>
<br />

<p>
Obtain the private IP address of the Ubuntu virtual machine and attempt to ping it from within the Windows 10 virtual machine using PowerShell. 
Once the ping command is entered, observe ping requests and replies within Wireshark.
</p>
<p>
<img src="https://i.imgur.com/4acUfOI.png" height="70%" width="70%" alt="Ping VM2"/>
</p>
<br />

<p>
After that, ping a website (in the example below, www.google.com) and watch the traffic in WireShark. 
</p>
<p>
<img src="https://i.imgur.com/n3ImMja.png" height="70%" width="70%" alt="Ping Website"/>
</p>
<br />

<p>
Use the command: ping (the private IP address of the Ubuntu virtual machine) -t to start a continuous ping from your Windows 10 virtual machine 
to your Ubuntu virtual machine, and then use Wireshark to watch the ping requests and replies.
</p>
<p>
<img src="https://i.imgur.com/ENH5Bpo.png" height="70%" width="70%" alt="Continuous VM 2 Ping"/>
</p>
<br />

<p>
Go to Azure, open the Network Security Group your Ubuntu virtual machine is using, and create a rule to disable incoming (inbound) ICMP traffic.
</p>
<p>
<img src="https://i.imgur.com/XlAZHtJ.png" height="70%" width="70%" alt="Deny ICMP"/>
</p>
<br />

<p>
Go back to the Windows 10 virtual machine and watch the ICMP traffic in Wireshark and the command-line ping activity in PowerShell. 
You should observe a timed-out ICMP request.
</p>
<p>
<img src="https://i.imgur.com/djYGb6S.png" height="70%" width="70%" alt="Timed Out ICMP Request"/>
</p>
<br />

<p>
Then go back to Azure and open the Network Security Group your Ubuntu virtual machine is using and allow ICMP traffic again. 
</p>
<p>
<img src="https://i.imgur.com/9gM3Qfc.png" height="70%" width="70%" alt="Allow ICMP"/>
</p>
<br />

<p>
Once this is complete, immediately go back to the Windows 10 virtual machine and watch the ICMP traffic in Wireshark, 
and the command-line ping activity within PowerShell should begin working.
</p>
<p>
<img src="https://i.imgur.com/1xmu9vt.png" height="70%" width="70%" alt="Reply ICMP"/>
</p>
<br />

<p>
Refresh Wireshark, filter for SSH traffic only, and then within PowerShell on your Windows 10 virtual machine, start an SSH connection to connect 
to your Ubuntu virtual machine via its private IP address.
</p>
<p>
<img src="https://i.imgur.com/O3yMCng.png" height="70%" width="70%" alt="SSH Allow Connection"/>
</p>
<br />

<p>
Shown below is a successful SSH connection within PowerShell, as well as the SSH traffic in Wireshark.
</p>
<p>
<img src="https://i.imgur.com/SIVInCE.png" height="70%" width="70%" alt="SSH VM2 Connected"/>
</p>
<br />

<p>
Once connected to the Ubuntu virtual machine, you can type commands (id, uname -a, pwd, etc.) in Linux within PowerShell and watch the SSH traffic spam in WireShark.
</p>
<p>
<img src="https://i.imgur.com/bZ3enYJ.png" height="70%" width="70%" alt="Commands"/>
</p>
<br />

<p>
After that, use the exit command to terminate the SSH connection. 
</p>
<p>
<img src="https://i.imgur.com/sEdNYvk.png" height="70%" width="70%" alt="SSH Closed Connection"/>
</p>
<br />

<p>
Refresh Wireshark, filter for DHCP traffic only, and then within PowerShell on your Windows 10 virtual machine, attempt to issue a new IP address from the command line by entering ipconfig /renew. 
Watch the DHCP traffic shown in Wireshark.
</p>
<p>
<img src="https://i.imgur.com/e2qBjJh.png" height="70%" width="70%" alt="DHCP"/>
</p>
<br />

<p>
Lastly, refresh Wireshark, filter for DNS traffic only, and then within PowerShell on your Windows 10 virtual machine, use the nslookup command for www.google.com and look at what google.com’s IP addresses are. 
Watch the DNS traffic shown in Wireshark.
</p>
<p>
<img src="https://i.imgur.com/P5czcyg.png" height="70%" width="70%" alt="DNS"/>
</p>
<br />
