<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (ICMP, SSH, DHCP, DNS, RDP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Linux (Ubuntu) Server 20.04

<h2>Actions and Observations</h2>

<h3>Internet Control Message Protocol (ICMP) Traffic</h3>

<p>
<img src="https://i.imgur.com/tQUL2Rm.png" height="80%" width="80%" alt="ICMP Traffic"/>
</p>
<p>
Upon opening Wireshark, click on Ethernet to start packet capture. In the search bar at the top, type in "icmp" to filter for ICMP traffic. ICMP is a type of message that computers use to communicate with each other over the internet or network. Go to the Start icon of your computer and search for Windows PowerShell. Type in ping 10.0.0.5 to reach the Linux virtual machine we created. The ping command uses ICMP traffic and tests to see if a computer or a server is reachable. In this case, we were able to reach the Linux server. In Wireshark & Powershell, we can see the Windows virtual machine sending four sets of request to Linux and Linux replying back to each one.
</p>
<br />

<p>
<img src="https://i.imgur.com/08EHqWj.png" height="80%" width="80%" alt="ICMP Traffic"/>
</p>
<p>
You can also use the ping command to check for Internet connnectivity. Here we ping www.google.com and it proves to be successful, meaning that the Internet connection is working.
</p>
<br />

<h3>Network Security Group (Firewall)</h3>

<p>
<img src="https://i.imgur.com/5eHqrrd.png" height="80%" width="80%" alt="Network Security Group"/>
</p>
<p>
Here, you can restart the packet capture by clicking on the green shark fin icon at the top by the stop icon. Click Continue without saving. In Windows Powershell, type ping 10.0.0.5 -t to start a perpetual ping from your Windows VM to your Linux VM.
</p>
<br />

<p>
<img src="https://i.imgur.com/xAw4r7T.png" height="80%" width="80%" alt="Network Security Group"/>
</p>
<p>
Now, you're going to open the Network Security Group your Linux VM is using and disable incoming/inbound ICMP traffic. Essentially, you are configuring a cloud firewall to block the connection coming from the Windows VM. Go to Microsoft Azure and click on your Linux VM.
</p>
<br />

<p>
<img src="https://i.imgur.com/GWGdREs.png" height="80%" width="80%" alt="Network Security Group"/>
</p>
<p>
Then go to Network settings. Follow along through the next couple of screenshots.
</p>
<br />

<p>
<img src="https://i.imgur.com/pUJST1G.png" height="80%" width="80%" alt="Network Security Group"/>
</p>
<br />

<p>
<img src="https://i.imgur.com/Af7awAJ.png" height="80%" width="80%" alt="Network Security Group"/>
</p>
<p>
<br />

<p>
<img src="https://i.imgur.com/O1VVnAf.png" height="80%" width="80%" alt="Network Security Group"/>
</p>
<p>
Click on Add to create a new inbound security rule.
</p>
<br />

<p>
<img src="https://i.imgur.com/8L553IV.png" height="80%" width="80%" alt="Network Security Group"/>
</p>
<p>
For destination port ranges, put *, which means any because ICMP doesn't use a port. For Protocol, select ICMPv4 because the ping command uses the ICMP protocol as mentioned earlier. Select Deny for Action to block incoming traffic.
</p>
<br />

<p>
<img src="https://i.imgur.com/ocZlm5y.png" height="80%" width="80%" alt="Network Security Group"/>
</p>
<p>
Scroll down put 290 for Priority. What this does is place a higher priority on the rule over all the other ones. Then click Add.
</p>
<br />

<p>
<img src="https://i.imgur.com/b7GzBiD.png" height="80%" width="80%" alt="Network Security Group"/>
</p>
<p>
Switch back to the Windows VM to observe the traffic. After a while, once the rule takes into effect, the traffic will stop. On Windows Powershell, you will start seeing Request timed out. Notice that in Wireshark you see (no response found) in the Info column. 
</p>
<br />

<p>
<img src="https://i.imgur.com/0S8rbfb.png" height="80%" width="80%" alt="Network Security Group"/>
</p>
<p>
Now go back to the Network Security Group on Azure and delete the rule we just created.
</p>
<br />

<p>
<img src="https://i.imgur.com/UZkRRvc.png" height="80%" width="80%" alt="Network Security Group"/>
</p>
<p>
It will take awhile again for the change to be implemented. You will see replies from the Linux server again.
</p>
<br />

<p>
<img src="https://i.imgur.com/18oW8lO.png" height="80%" width="80%" alt="Network Security Group"/>
</p>
<p>
You can stop the ping activity by pressing ctrl+C.
</p>
<br />

<h3>Secure Shell (SSH) Traffic</h3>

<p>
<img src="https://i.imgur.com/BGeGWXO.png" height="80%" width="80%" alt="SSH"/>
</p>
<p>
Start a new packet capture in Wireshark by clicking on Ethernet.
</p>
<br />

<p>
<img src="https://i.imgur.com/yGQZmA6.png" height="80%" width="80%" alt="SSH"/>
</p>
<p>
You can either type ssh or tcp.port==22 in the bar up top to filter for Secure Shell traffic. Open PowerShell and type ssh labuser@10.0.0.5. Using the ssh command helps us to connect to the Linux server securely over the network. Answer yes to the question "Are you sure you want to continue connecting (yes/no/[fingerprint])?" We are prompted to enter the Linux server's password. Enter the password and note that you won't see it being typed out for security purposes. 
</p>
<br />

<p>
<img src="https://i.imgur.com/bpwSHqD.png" height="80%" width="80%" alt="SSH"/>
</p>
<p>
Now we are connected to the Linux VM. Notice in Wireshark that each data packet are encrypted, making it difficult for hackers to intercept and read the information.
</p>
<br />

<p>
<img src="https://i.imgur.com/mZye1bU.png" height="80%" width="80%" alt="SSH"/>
</p>
<p>
The commands typed here are just to verify that we are connected to the VM. The id command shows our user id. The hostname command shows us the name of the server or computer that we're connected to. Lastly, the uname -a command displays information of the operating system.
</p>
<br />

<p>
<img src="https://i.imgur.com/WSaxH7E.png" height="80%" width="80%" alt="SSH"/>
</p>
<p>
To end the SSH connection, type exit in the command line and you will see that the prompt has changed back to the Windows VM.
</p>
<br />

<h3>Dynamic Host Configuration Protocol (DHCP) Traffic</h3>

<p>
<img src="https://i.imgur.com/PYji3S3.png" height="80%" width="80%" alt="DHCP"/>
</p>
<p>
In this exercise, we will observe DHCP traffic. DHCP is a network protocol that automatically assigns IP addresses and other network settings to devices when they connect to a network. Start a new packet capture in Wireshark and filter for dhcp. You can also type udp.port ==67 || udp.port ==68. We are attempting to issue the Windows VM a new IP address from Powershell. Open Notepad and type what you see in the screenshot. 
</p>
<br />

<p>
<img src="https://i.imgur.com/yPDti63.png" height="80%" width="80%" alt="DHCP"/>
</p>
<p>
Go to File and click Save As. We are saving this text file in Program Data on our VM so in the bar up top, type c:\programdata and press Enter.
</p>
<br />

<p>
<img src="https://i.imgur.com/hSqBO2Y.png" height="80%" width="80%" alt="DHCP"/>
</p>
<p>
Save the file as dhcp.bat.
</p>
<br />

<p>
<img src="https://i.imgur.com/FRyxksR.png" height="80%" width="80%" alt="DHCP"/>
</p>
<p>
In Powershell, type cd c:\programdata to change the directory to Program Data. Type ls to access the list,ensuring that the text file is saved.
</p>
<br />

<p>
<img src="https://i.imgur.com/gz2BhIR.png" height="80%" width="80%" alt="DHCP"/>
</p>
<p>
Enter the command .\dhcp.bat to run the script. Nothing will happen at first, but a few moments later, the remote connection to the Windows VM will disconnect and re-connect. When the connection is restored, this is what you will see. During this process, the computer released its IP address in favor for a new one from DHCP. In this case, the Windows VM requested the IP address that it had before.
</p>
<br />

<p>
<img src="https://i.imgur.com/TydXBHJ.png" height="80%" width="80%" alt="DHCP"/>
</p>
<p>
Wireshark has captured the DHCP process. When ipconfig /release was executed, the Windows VM (10.0.0.4) sent a release packet to the Azure's DHCP server (168.63.129.16). When ipconfig /renew was executed, that's when the Discover, Offer, Request, and Acknowledge packets were sent over the Internet. Discover is when a device (client) sends a broadcast (255.255.255.255) to find a DHCP server. The Windows VM has a source address of 0.0.0.0 since it has no IP address. Offer is when the DHCP server responds with an offer, including an IP address. Lastly, the server confirms the assignment and the client can use the IP with the Acknowlege packet.
</p>
<br />

<h3>DNS Traffic</h3>
<p>
<img src="https://i.imgur.com/Gcn8OMe.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/BsulkDA.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<h3>RDP Traffic</h3>
<p>
<img src="https://i.imgur.com/tdTcJIJ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />
