<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we use Wireshark to observe various network traffic to and from Azure Virtual Machines and experiment with Network Security Groups. <br />


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

<h3>Creating Virtual Machines</h3>

<p>
<img src="https://i.imgur.com/OqAVhfl.png" height="80%" width="80%" alt="Creating VMs"/>
</p>
<p>
To begin this tutorial, you need to create a Windows 10 VM and a Linux VM. In Microsoft Azure, go to Resource groups.
</p>
<br />

<p>
<img src="https://i.imgur.com/Ag6UfAx.png" height="80%" width="80%" alt="Creating VMs"/>
</p>
<p>
Here, I've already created my resource group. To make one, click on Create.
</p>
<br />

<p>
<img src="https://i.imgur.com/YXRkEOY.png" height="80%" width="80%" alt="Creating VMs"/>
</p>
<p>
Name your resource group to anything you want and set the region to wherever region you live in. Click on Review + create.
</p>
<br />

<p>
<img src="https://i.imgur.com/X8HNU8M.png" height="80%" width="80%" alt="Creating VMs"/>
</p>
<p>
Click Create.
</p>
<br />

<p>
<img src="https://i.imgur.com/fFlQxL0.png" height="80%" width="80%" alt="Creating VMs"/>
</p>
<p>
Next, you have to create the virtual machines. Click on the bar at top, search for Virtual machines, and click on it.
</p>
<br />

<p>
<img src="https://i.imgur.com/qTgCtDw.png" height="80%" width="80%" alt="Creating VMs"/>
</p>
<p>
Go to Create and select Virtual machine.
</p>
<br />

<p>
<img src="https://i.imgur.com/7qSomGd.png" height="80%" width="80%" alt="Creating VMs"/>
</p>
<p>
The subscription you have with Azure should already be selected. For resource group, choose the one you've just created.
</p>
<br />

<p>
<img src="https://i.imgur.com/SIr4bs8.png" height="80%" width="80%" alt="Creating VMs"/>
</p>
<p>
Name your virtual machine. Here, I am creating the Windows 10 VM first. Choose the same region you used when creating the resource group.
</p>
<br />

<p>
<img src="https://i.imgur.com/0aVbzCB.png" height="80%" width="80%" alt="Creating VMs"/>
</p>
<p>
Choose the Windows 10 Pro image for the virtual machine.
<br />

<p>
<img src="https://i.imgur.com/iE17g3D.png" height="80%" width="80%" alt="Creating VMs"/>
</p>
<p>
For size, choose one that has at least 2 vcpus. 
</p>
<br />

<p>
<img src="https://i.imgur.com/zBFXYII.png" height="80%" width="80%" alt="Creating VMs"/>
</p>
<p>
Create an administrator account for when you log into the virtual machine.
</p>
<br />

<p>
<img src="https://i.imgur.com/xGuzMYi.png" height="80%" width="80%" alt="Creating VMs"/>
</p>
<p>
Make sure to check the box listed under Licensing. Click Next.
</p>
<br />

<p>
<img src="https://i.imgur.com/UBytmE5.png" height="80%" width="80%" alt="Creating VMs"/>
</p>
<p>
Leave everything as is and click Next to Networking.
</p>
<br />

<p>
<img src="https://i.imgur.com/y8d16Ob.png" height="80%" width="80%" alt="Creating VMs"/>
</p>
<p>
Go to Create new for your Virtual Network.
</p>
<br />

<p>
<img src="https://i.imgur.com/lZqRqoQ.png" height="80%" width="80%" alt="Creating VMs"/>
</p>
<p>
Name your virtual network and click OK.
</p>
<br />

<p>
<img src="https://i.imgur.com/DTsMfdU.png" height="80%" width="80%" alt="Creating VMs"/>
</p>
<p>
Go to Review + create.
</p>
<br />

<p>
<img src="https://i.imgur.com/UtLb3SZ.png" height="80%" width="80%" alt="Creating VMs"/>
</p>
<p>
Click Create.
</p>
<br />

<p>
<img src="https://i.imgur.com/P1FMCEs.png" height="80%" width="80%" alt="Creating VMs"/>
</p>
<p>
Wait for the deployment to be created.
</p>
<br />

<p>
<img src="https://i.imgur.com/5QYyiR8.png" height="80%" width="80%" alt="Creating VMs"/>
</p>
<p>
Now that the Windows 10 VM is created, you need to create the Linux VM. Just go to Create another VM.
</p>
<br />

<p>
<img src="https://i.imgur.com/1fHTJEJ.png" height="80%" width="80%" alt="Creating VMs"/>
</p>
<p>
Choose the same resource group as before and name the Linux VM.
</p>
<br />

<p>
<img src="https://i.imgur.com/Nges0jN.png" height="80%" width="80%" alt="Creating VMs"/>
</p>
<p>
Select the image as seen in the screenshot.
</p>
<br />

<p>
<img src="https://i.imgur.com/0lsdLi2.png" height="80%" width="80%" alt="Creating VMs"/>
</p>
<p>
For size, choose the same one for the Windows 10 VM.
</p>
<br />

<p>
<img src="https://i.imgur.com/OaTtehW.png" height="80%" width="80%" alt="Creating VMs"/>
</p>
<p>
Change the Authentication type from SSH public key to Password and create your administration account. You can use the same one you used for the Windows 10 VM. Click Next to Disks.
</p>
<br />

<p>
<img src="https://i.imgur.com/lFNiT8m.png" height="80%" width="80%" alt="Creating VMs"/>
</p>
<p>
As before, leave everything as is and go to Next for Networking.
</p>
<br />

<p>
<img src="https://i.imgur.com/gLghbXe.png" height="80%" width="80%" alt="Creating VMs"/>
</p>
<p>
Ensure that the virtual network you created is already selected. Click Review + create.
</p>
<br />

<p>
<img src="https://i.imgur.com/0cYCkQq.png" height="80%" width="80%" alt="Creating VMs"/>
</p>
<p>
Create the virtual machine.
</p>
<br />

<p>
<img src="https://i.imgur.com/uLjjZfP.png" height="80%" width="80%" alt="Creating VMs"/>
</p>
<p>
Wait for the deployment to be completed.
</p>
<br />

<p>
<img src="https://i.imgur.com/pEWO5vB.png" height="80%" width="80%" alt="Creating VMs"/>
</p>
<p>
Now go back to Virtual machines using the search bar.
</p>
<br />

<p>
<img src="https://i.imgur.com/00QiYkW.png" height="80%" width="80%" alt="Creating VMs"/>
</p>
<p>
Go to Start on your computer, search for Remote Desktop Connection and open it.
</p>
<br />

<p>
<img src="https://i.imgur.com/XnuXoTw.png" height="80%" width="80%" alt="Creating VMs"/>
</p>
<p>
Copy and paste the public IP address of the Windows 10 VM and go to Connect.
</p>
<br />

<p>
<img src="https://i.imgur.com/LymJI9K.png" height="80%" width="80%" alt="Creating VMs"/>
</p>
<p>
Go to More choices.
</p>
<br />

<p>
<img src="https://i.imgur.com/RuAuh2m.png" height="80%" width="80%" alt="Creating VMs"/>
</p>
<p>
Click on Use a different account.
</p>
<br />

<p>
<img src="https://i.imgur.com/zqjq55W.png" height="80%" width="80%" alt="Creating VMs"/>
</p>
<p>
Use the administration account you've made when creating the virtual machines.
</p>
<br />

<p>
<img src="https://i.imgur.com/RQBqEik.png" height="80%" width="80%" alt="Creating VMs"/>
</p>
<p>
Click Yes and you'll be remotely connected to your virtual machine.
</p>
<br />

<h3>Installing Wireshark</h3>

<p>
<img src="https://i.imgur.com/b1d9Jow.png" height="80%" width="80%" alt="Wireshark Installation"/>
</p>
<p>
Go to http://www.wireshark.org and click on Download.
</p>
<br />

<p>
<img src="https://i.imgur.com/A1V9F4k.png" height="80%" width="80%" alt="Wireshark Installation"/>
</p>
<p>
Select Windows x64 Installer and the file will start downloading.
</p>
<br />

<p>
<img src="https://i.imgur.com/rkEgFpW.png" height="80%" width="80%" alt="Wireshark Installation"/>
</p>
<p>
Once the download is finished, go to the top right corner and open the file.
</p>
<br />

<p>
<img src="https://i.imgur.com/4trI1y0.png" height="80%" width="80%" alt="Wireshark Installation"/>
</p>
<p>
Click Next to continue. 
</p>
<br />

<p>
<img src="https://i.imgur.com/fOJEsVD.png" height="80%" width="80%" alt="Wireshark Installation"/>
</p>
<p>
Click Noted.
</p>
<br />

<p>
<img src="https://i.imgur.com/lGxvVEC.png" height="80%" width="80%" alt="Wireshark Installation"/>
</p>
<p>
Click Next.
</p>
<br />

<p>
<img src="https://i.imgur.com/sSz6wK2.png" height="80%" width="80%" alt="Wireshark Installation"/>
</p>
<p>
Leave everything as is and click Next.
</p>
<br />

<p>
<img src="https://i.imgur.com/pZQpCrB.png" height="80%" width="80%" alt="Wireshark Installation"/>
</p>
<p>
If you like, you could also check the box for Wireshark Desktop Icon like how I did here. Click Next.
</p>
<br />

<p>
<img src="https://i.imgur.com/Lu9nMQa.png" height="80%" width="80%" alt="Wireshark Installation"/>
</p>
<p>
Click Next.
</p>
<br />

<p>
<img src="https://i.imgur.com/Rjd6Wr7.png" height="80%" width="80%" alt="Wireshark Installation"/>
</p>
<p>
Enusre that the box is checked for Install Npcap 1.79 and click Next.
</p>
<br />

<p>
<img src="https://i.imgur.com/4GQpack.png" height="80%" width="80%" alt="Wireshark Installation"/>
</p>
<p>
Click Install.
</p>
<br />

<p>
<img src="https://i.imgur.com/i4K3y19.png" height="80%" width="80%" alt="Wireshark Installation"/>
</p>
<p>
Select I Agree to the License Agreement.
</p>
<br />

<p>
<img src="https://i.imgur.com/MX0DFEi.png" height="80%" width="80%" alt="Wireshark Installation"/>
</p>
<p>
Click Install.
</p>
<br />

<p>
<img src="https://i.imgur.com/2D2htNp.png" height="80%" width="80%" alt="Wireshark Installation"/>
</p>
<p>
Once the installation is completed, proceed forward.
</p>
<br />

<p>
<img src="https://i.imgur.com/3AnHe1F.png" height="80%" width="80%" alt="Wireshark Installation"/>
</p>
<p>
Click Finish.
</p>
<br />

<p>
<img src="https://i.imgur.com/gzjjGlQ.png" height="80%" width="80%" alt="Wireshark Installation"/>
</p>
<p>
Click Next.
</p>
<br />

<p>
<img src="https://i.imgur.com/OQt5yVk.png" height="80%" width="80%" alt="Wireshark Installation"/>
</p>
<p>
Click Finish to close Setup.
</p>
<br />

<p>
<img src="https://i.imgur.com/J8pTOLn.jpeg" height="80%" width="80%" alt="Wireshark Installation"/>
</p>
<p>
Wireshark should now be on your desktop.
</p>
<br />

<h3>Observing ICMP Traffic</h3>

<p>
<img src="https://i.imgur.com/oa6Tptz.png" height="80%" width="80%" alt="ICMP Traffic"/>
</p>
<p>
Upon opening Wireshark, click on Ethernet to start packet capture.
</p>
<br />

<p>
<img src="https://i.imgur.com/OaqqOKC.png" height="80%" width="80%" alt="ICMP Traffic"/>
</p>
<p>
In the search bar at the top, type in "icmp" to filter for ICMP traffic.
</p>
<br />

<p>
<img src="https://i.imgur.com/JQkddiE.png" height="80%" width="80%" alt="ICMP Traffic"/>
</p>
<p>
Go to the Start icon of your computer and search for Windows PowerShell. Open it and type in ping 10.0.0.5 to reach the Linux virtual machine.
</p>
<br />

<p>
<img src="https://i.imgur.com/tQUL2Rm.png" height="80%" width="80%" alt="ICMP Traffic"/>
</p>
<p>
As you can see, we were able to reach the Linux server.
</p>
<br />

<p>
<img src="https://i.imgur.com/08EHqWj.png" height="80%" width="80%" alt="ICMP Traffic"/>
</p>
<p>You can also use the ping command to check for Internet connectivity. Here, we ping www.google.com, and it proves successful, meaning that the Internet connection is working.
</p>
<br />

<h3>Network Security Group (Firewall)</h3>

<p>
<img src="https://i.imgur.com/xxykVl5.png" height="80%" width="80%" alt="Network Security Group"/>
</p>
<p>
Here, you can restart the packet capture by clicking on the green shark fin icon at the top by the stop icon.
</p>
<br />

<p>
<img src="https://i.imgur.com/w1DlXFR.png" height="80%" width="80%" alt="Network Security Group"/>
</p>
<p>
Click Continue without saving.
</p>
<br />

<p>
<img src="https://i.imgur.com/5eHqrrd.png" height="80%" width="80%" alt="Network Security Group"/>
</p>
<p>
In Windows Powershell, type ping 10.0.0.5 -t to start a perpetual ping from your Windows VM to your Linux VM.
</p>
<br />

<p>
<img src="https://i.imgur.com/xAw4r7T.png" height="80%" width="80%" alt="Network Security Group"/>
</p>
<p>
While the perpetual ping is going, you will open your Linux VM's Network Security Group and disable incoming/inbound ICMP traffic. Back in Microsoft Azure on your personal desktop, click on virtual machines and then double-click on your Linux VM.
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
For destination port ranges, put *, which means any because ICMP doesn't use a port. For Protocol, select ICMPv4. For Action, select Deny to block incoming traffic.
</p>
<br />

<p>
<img src="https://i.imgur.com/ocZlm5y.png" height="80%" width="80%" alt="Network Security Group"/>
</p>
<p>
Scroll down and enter 290 for Priority. This will make the rule a higher priority than all the others. Then click Add.
</p>
<br />

<p>
<img src="https://i.imgur.com/b7GzBiD.png" height="80%" width="80%" alt="Network Security Group"/>
</p>
<p>
Switch back to the Windows VM to observe the traffic. After a while, once the rule takes effect, the traffic will stop. On Windows Powershell, you will start seeing Request timed out. Notice that you see (no response found) in the Info column in Wireshark. 
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
The change will again take some time to implement. You will see replies from the Linux server again.
</p>
<br />

<p>
<img src="https://i.imgur.com/18oW8lO.png" height="80%" width="80%" alt="Network Security Group"/>
</p>
<p>
You can stop the ping activity by pressing ctrl+C.
</p>
<br />

<h3>Observing SSH Traffic</h3>

<p>
<img src="https://i.imgur.com/BGeGWXO.png" height="80%" width="80%" alt="SSH"/>
</p>
<p>
Start a new packet capture in Wireshark by clicking on Ethernet.
</p>
<br />

<p>
<img src="https://i.imgur.com/qFv2PMO.png" height="80%" width="80%" alt="SSH"/>
</p>
<p>
Type ssh in the bar up top to filter for Secure Shell traffic.
</p>
<br />

<p>
<img src="https://i.imgur.com/UcqIrVC.png" height="80%" width="80%" alt="SSH"/>
</p>
<p>
Open PowerShell and type ssh labuser@10.0.0.5 to connect to the Linux VM.
</p>
<br />

<p>
<img src="https://i.imgur.com/Q5gCAzL.png" height="80%" width="80%" alt="SSH"/>
</p>
<p>
Answer yes to the question, "Are you sure you want to continue connecting (yes/no/[fingerprint])?"
</p>
<br />

<p>
<img src="https://i.imgur.com/irG9rAB.png" height="80%" width="80%" alt="SSH"/>
</p>
<p>
Next, enter the Linux server's password. Enter the password; note that you won't see it typed out for security purposes.
</p>
<br />

<p>
<img src="https://i.imgur.com/bpwSHqD.png" height="80%" width="80%" alt="SSH"/>
</p>
<p>
Now we are connected to the Linux VM. In Wireshark, notice that each data packet is encrypted, making it difficult for hackers to intercept and read the information.
</p>
<br />

<p>
<img src="https://i.imgur.com/nf1XEKK.png" height="80%" width="80%" alt="SSH"/>
</p>
<p>
The following commands typed here are just to verify that you are connected to the VM. The id command shows our user ID.
</p>
<br />

<p>
<img src="https://i.imgur.com/LJOdkUy.png" height="80%" width="80%" alt="SSH"/>
</p>
<p>
The hostname command shows the name of the server or computer that we're connected to.
</p>
<br />

<p>
<img src="https://i.imgur.com/Kgj030n.png" height="80%" width="80%" alt="SSH"/>
</p>
<p>
Lastly, the uname -a command displays information about the operating system.
</p>
<br />

<p>
<img src="https://i.imgur.com/WSaxH7E.png" height="80%" width="80%" alt="SSH"/>
</p>
<p>
To end the SSH connection, type exit in the command line. The prompt will then change back to the Windows VM.
</p>
<br />

<h3>Observing DHCP Traffic</h3>

<p>
<img src="https://i.imgur.com/PYji3S3.png" height="80%" width="80%" alt="DHCP"/>
</p>
<p>
Now, we will observe DHCP traffic. Start a new packet capture in Wireshark and filter for DHCP. We are attempting to issue a new IP address for the Windows VM from Powershell. Open Notepad and type what you see in the screenshot. 
</p>
<br />

<p>
<img src="https://i.imgur.com/yPDti63.png" height="80%" width="80%" alt="DHCP"/>
</p>
<p>
Go to File and click Save As. We are saving this text file in Program Data on our VM, so in the bar up top, type c:\programdata and press Enter.
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
In Powershell, type cd c:\programdata to change the directory to Program Data. Type ls to access the list, ensuring the text file is saved.
</p>
<br />

<p>
<img src="https://i.imgur.com/gz2BhIR.png" height="80%" width="80%" alt="DHCP"/>
</p>
<p>
Enter the command .\dhcp.bat to run the script. Nothing will happen at first, but a few moments later, the remote connection to the Windows VM will disconnect and reconnect. This is what you will see when the connection is restored. During this process, the computer released its IP address in favor of a new one from DHCP. In this case, the Windows VM requested the IP address it had before.
</p>
<br />

<p>
<img src="https://i.imgur.com/TydXBHJ.png" height="80%" width="80%" alt="DHCP"/>
</p>
<p>
Wireshark has captured the DHCP process. The computer executed the ipconfig /release command and sent a release packet to Azure's DHCP server (168.63.129.16). When ipconfig /renew was executed, the Discover, Offer, Request, and Acknowledge packets were sent over the Internet.
</p>
<br />

<h3>Observing DNS Traffic</h3>
<p>
<img src="https://i.imgur.com/Gcn8OMe.png" height="80%" width="80%" alt="DNS"/>
</p>
<p>
This activity aims to give you an idea of how DNS works. Start a new packet capture and filter for DNS. The nslookup command examines DNS servers and finds the IP address associated with a domain name or vice versa. For example, you can use nslookup to find Disney's IP address.
</p>
<br />

<p>
<img src="https://i.imgur.com/BsulkDA.png" height="80%" width="80%" alt="DNS"/>
</p>
<p>
When you try to enter the IP address in an Internet browser, it will not go through because the Internet only recognizes domain names, but you can tell that the IP belongs to Disney.
</p>
<br />

<h3>Observing RDP Traffic</h3>
<p>
<img src="https://i.imgur.com/tdTcJIJ.png" height="80%" width="80%" alt="RDP"/>
</p>
<p>
Here, we observe RDP traffic, which we use to connect to the Windows VM. You will see non-stop traffic once you start a new packet capture and filter for RDP. The RDP protocol constantly shows you a live stream from one computer to another, meaning traffic is always transmitted.
</p>
<br />
