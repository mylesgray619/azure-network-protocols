<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Create A Virtual Machine (VM)
- Observe ICMP Traffic
- Configuring A Firewall (Network Security Group)
- Observe SSH Traffic
- Observe DHCP Traffic
- Observe DNS Traffic
- Observe RDP traffic

<h2>Actions and Observations</h2>

<p>
<img src="https://i.imgur.com/HypLqan.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Here Im going to focus on creating two virtual machines use microsoft azure on https://portal.azure.com. Our first step is to create a resource group which is like a folder thats gonna hold our virtual machines for easier managment.
</p>
<br />

<p>
<img src="https://i.imgur.com/d8mhYEt.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/8SwyvuF.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next Im going to create our first virtual machine by choosing the resource group we just made. This VM is gonna operate on windows 10. Creating this VM is going to allow Azure to also create a new Virtual Network (VNet) and subnet.
</p>
<br />

<p>
<img src="https://i.imgur.com/DPi2mym.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next Im going to create my second VM using linux (ubuntu) operating system and im going use the same resource group and same virtual network that I used for windows 10. This ensures that both VMs are on the same network and can communicate. Lastly im gonna  choose a username/password to login for both VMs and make sure theyre both in the same VNet and Subnet.
</p>
<br />

<p>
<img src="https://i.imgur.com/q1aNaxk.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/aLeXusS.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Here Im going to use wireshark inside my windows 10 VM to observe ICMP traffic. To start im connecting to my windows 10 VM using remote desktop. Afterwards inside the VM I downloaded and installed wireshark.
</p>
<br />

<p>
<img src="https://i.imgur.com/1ajwXly.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next I started packet capture in wireshark to begin monitoring network traffic. I applied a filter to only show ICMP traffic. As you can see there is no traffic going on in the capture yet.
</p>
<br />

<p>
<img src="https://i.imgur.com/CJc1Oju.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
To see any traffic going on I went onto the linux VM to find its private IP address. I copied the private IP address and opened power shell to paste, then ping the linux VM from the windows VM.  This will generate ICMP traffic and I can observe the ping request an replies.
</p>
<br />

<p>
<img src="https://i.imgur.com/HtUB1uv.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/mwMDkHI.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Here Im showing how to configure a firewall using network security group (NSG). To start I typed "ping 10.0.0.5 -t" in powershell to intiate a perpetual ping from the windows 10 VM to the ubuntu VM. This sends a constant ICMP traffic so I can monitor it in real time.
</p>
<br />

<p>
<img src="https://i.imgur.com/N2n1leG.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/GhidlkI.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next Im going to open up the network security group on the linux VM, then add a security rule to disable incoming (inbound) ICMP traffic. Now I can go back onto the remote desktop inside the windows VM to observe the ping request being blocked by a firewall.
</p>
<br />

<p>
<img src="https://i.imgur.com/txM30gg.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/DO6DPlp.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next Im going to re-enable ICMP traffic for the network security group by going back into the linux VM on Azure, then deleting the security rule that was placed to stop the traffic.
</p>
<br />

<p>
<img src="https://i.imgur.com/asrzMYf.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Here Im going to observe SSH traffic by applying a SSH filter to see related packets. Afterwards im going to open up powershell and connect to the linux VM using its private IP address. In order to connect im going to type in SSH labuser@10.0.05 to log into the linux VM then type ion the password was created for it. This allows me to have access to the linux VM through my windows VM.
</p>
<br />

<p>
<img src="https://i.imgur.com/XZFbCMR.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Here Im going to observe DHCP traffic by attempting to issue the windows VM a new IP address from the command line. How I did this I created a script that has two commands, one called ipconfig /release and ipconfig /renew, to a file. I save that file to c:\programdata and title that file dhcp.bat. Afterwards I type in the command line on powershell ".\dhcp.bat." This will immediately execute the release command and the script will automatically execute ipconfig /renew which will allow me to see traffic on the packet capture.
</p>
<br />

<p>
<img src="https://i.imgur.com/L6AxqDL.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Here is a quick fun session of observing DNS traffic by typing in nslookup then observing the IP addresses for disney and google.com
</p>
<br />

<p>
<img src="https://i.imgur.com/SB8OXVi.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Here Im observing RDP traffic by typing in tcp.port==3389. Doing this I can  immediately see non-stop spamming of traffic. Reason being is because the RDP is constantly showing me a live stream from one computer to another.
</p>
<br />
