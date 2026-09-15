Azure & Wireshark Networking Lab
Virtual Machines, ICMP, SSH, DHCP, DNS, RDP, and Network Security Groups
Lab Objectives


1. Create and configure Azure virtual machines.
2. Understand Azure Resource Groups, VNets, and Subnets.
3. Place two VMs on the same virtual network.
4. Connect to a Windows VM using Remote Desktop.
5. Use Wireshark to capture and filter network traffic.
6. Observe ICMP, SSH, DHCP, DNS, and RDP traffic.
7. Configure an Azure Network Security Group (NSG).
8. Observe how firewall rules affect network traffic.
9. Properly clean up Azure resources to prevent unnecessary charges.
Part 1 — Create the Virtual Machines
Step 1 — Create a Resource Group

What you are doing:
Sign in to the Azure Portal and create a Resource Group that will contain the virtual machines and their associated networking resources. A Resource Group provides a way to organize and manage related Azure resources together.

Procedure:

1. Sign in to the Azure Portal.
2. Search for Resource groups.
3. Select Resource groups.
4. Click + Create.
5. Select your Azure subscription.


Enter a name such as:

network-lab-rg

Select an Azure region.
Click Review + create.
Select Create.
<img width="1047" height="667" alt="image" src="https://github.com/user-attachments/assets/c5340729-7e26-4084-86f3-b18156eaf335" />

<img width="619" height="592" alt="image" src="https://github.com/user-attachments/assets/2adb7a63-8ab5-40a1-a0d6-a446af4cc694" />

<img width="800" height="451" alt="image" src="https://github.com/user-attachments/assets/88036869-eae5-4e40-b9b6-f144fddc078c" />



Step 2 — Create the Windows 10 Virtual Machine

What you are doing:
Create the Windows VM inside the Resource Group created in Step 1. During VM creation, Azure will also create a Virtual Network and Subnet that will later be reused by the Ubuntu VM.

Important: Depending on the current Azure Marketplace availability, Windows 10 may not appear as an available image in every subscription. If your course specifically requires Windows 10, use the Windows 10 image supplied/approved by your instructor.

Configure the VM
1. Search for Virtual machines.
2. Select Virtual machines.
3. Click + Create → Azure virtual machine.

4. Under Resource group, select:

network-lab-rg

5. Give the VM a name such as:

windows-vm

6. Select the required Windows 10 image.
7. Select an appropriate VM size.
8. Create the administrator username and password.
9. Under Public inbound ports, allow RDP (3389) so you can remotely connect to the VM.

Azure automatically creates a network interface for a VM and associates it with a subnet during portal-based VM creation.

Networking

10. Go to the Networking tab.

For the first VM:

Virtual network: Create new
Subnet: Create/default
Public IP: Create
Inbound port: RDP (3389)

Use a descriptive name such as:

lab-vnet

and the default subnet:

default

<img width="800" height="379" alt="image" src="https://github.com/user-attachments/assets/acb0ceb2-418a-465a-9ac0-3021c889e1a0" />

<img width="782" height="899" alt="image" src="https://github.com/user-attachments/assets/1d92083a-dcf8-4637-81d5-36f5ce5d5b1d" />

<img width="1200" height="697" alt="image" src="https://github.com/user-attachments/assets/606b1f6d-050a-4011-b694-be1a5d86de2c" />

<img width="850" height="559" alt="image" src="https://github.com/user-attachments/assets/170c5c07-c888-4481-999c-4a076ece3030" />

<img width="694" height="437" alt="image" src="https://github.com/user-attachments/assets/b2ef2fbf-ee36-4f91-879d-3086193a85d0" />

Select Review + create.

Wait for Azure validation to pass.



Step 3 — Create the Linux/Ubuntu VM

What you are doing:
Create a second VM running Ubuntu. The critical part of this step is selecting the existing Resource Group, Virtual Network, and Subnet created for the Windows VM. This places both computers on the same Azure network.

Microsoft's current portal workflow supports selecting an existing Resource Group and configuring the VM's networking through the Networking tab.

Configure the VM
1. Return to Virtual machines.
2. Select + Create → Azure virtual machine.

3. Select the same Resource Group:

network-lab-rg

Name the VM:

linux-vm

4. Select an Ubuntu LTS image.

For example:

Ubuntu Server 24.04 LTS

5. Select an appropriate VM size.
Authentication

Under Administrator account, select:

Authentication type → Password

Enter:

Username: labuser
Password: create a secure lab password

Azure supports password authentication for Linux VMs, although Microsoft recommends stronger authentication methods such as SSH keys for production environments.

Networking

6. Go to the Networking tab.

This is the most important part of the VM configuration.

Set:

Virtual network: lab-vnet
Subnet: default

Do NOT create a new Virtual Network.

Click Create.


<img width="782" height="899" alt="image" src="https://github.com/user-attachments/assets/03d585c8-1426-4f5e-a8f3-3c80e5ef50bd" />


<img width="850" height="559" alt="image" src="https://github.com/user-attachments/assets/ffdbc16e-9756-420b-90ed-8e7979072594" />


<img width="712" height="850" alt="image" src="https://github.com/user-attachments/assets/d9895d5e-3255-4300-89e9-3f289d4230a1" />


7. Complete the remaining settings.

8. Select Review + create.

9. Select Create.


Step 4 — Verify Both VMs Are on the Same Virtual Network/Subnet

What you are doing:
Before beginning the packet-capture portion of the lab, verify that both VMs are connected to the same VNet and subnet. Azure VNets provide the network environment through which Azure resources can communicate.

Open:

1. Windows VM → Networking

and then:

2. Linux VM → Networking

Verify that both show:

Virtual Network: lab-vnet

Subnet: default


Step 5 — End Part 1

What you are doing:
At this point, the infrastructure portion of the lab is complete. Do not delete the VMs or Resource Group, because Part 2 uses the same machines.

You can stop working for now, but leave the resources available for the next section.

Do NOT perform the final cleanup yet.

Part 2 — Observe ICMP Traffic
Step 6 — Install Microsoft Remote Desktop if Using a Mac

What you are doing:
Mac users need an RDP client to connect to the Windows VM. Microsoft Remote Desktop allows you to establish a graphical Windows session from your Mac.

Windows users can use the built-in Remote Desktop Connection application.

Step 7 — Connect to the Windows VM Using Remote Desktop

What you are doing:
Use the Windows VM's public IP address and RDP to remotely access the Windows computer.

In Azure:

1. Open windows-vm.
2. Select Overview.
3. Find the Public IP address.
4. Copy the address.

On Windows:

1. Search for Remote Desktop Connection.
2. Enter the public IP.
3. Select Connect.
4. Enter your Azure VM administrator credentials.

On macOS, open Microsoft Remote Desktop and create a new PC connection using the Windows VM's public IP.

Security reminder: RDP exposed to the public Internet should be restricted in real environments. For a classroom lab, follow your instructor's configuration.

Step 8 — Install Wireshark

What you are doing:
Wireshark is a packet-analysis application that allows you to see network packets entering and leaving the Windows VM.

Download Wireshark from the official website:

Wireshark.org

Install Wireshark using the default installation options.

Step 9 — Start a Wireshark Packet Capture

What you are doing:
Start a live capture on the Windows VM's active network interface. Wireshark will begin displaying packets as they are transmitted.

Open Wireshark.
1. Identify the active network interface.
2. Double-click the interface.
3. Packets should begin appearing.

You will initially see a large amount of traffic.

Step 10 — Filter for ICMP Traffic

What you are doing:
Use a Wireshark display filter to show only ICMP packets. This makes it much easier to observe the packets generated by the ping command.

In the Wireshark filter bar, enter: icmp


<img width="961" height="605" alt="image" src="https://github.com/user-attachments/assets/61d27d54-61fb-4924-98fc-b97d62a76cac" />

<img width="980" height="587" alt="image" src="https://github.com/user-attachments/assets/bc070485-a29f-41d2-aeb1-67156b70501d" />

<img width="1040" height="885" alt="image" src="https://github.com/user-attachments/assets/c9422eb0-1de4-405f-869d-8ef2da24061a" />

<img width="753" height="588" alt="image" src="https://github.com/user-attachments/assets/87662c1f-e466-4720-8063-39f8ab4f16d1" />

<img width="1400" height="910" alt="image" src="https://github.com/user-attachments/assets/8dbe04e3-b148-4929-8f3d-22ed16003b71" />



Step 11 — Ping the Ubuntu VM

What you are doing:
Find the private IP address of linux-vm in Azure and use that address to send ICMP echo requests from the Windows VM.

Find the Linux private IP

Azure Portal:

linux-vm → Networking

Locate:

Private IP address

For example: 10.0.0.5
(Your address will be different.)

Ping the Linux VM

1. Inside the Windows VM, open PowerShell and enter: Ping 10.0.0.5
2. Replace 10.0.0.5 with your Ubuntu VM's actual private IP.
3. You should see: Reply from 10.0.0.5: bytes=32 time<1ms TTL=64
4. At the same time, Wireshark should display ICMP traffic.

<img width="768" height="426" alt="image" src="https://github.com/user-attachments/assets/f4193a32-dc27-4bfa-8204-1ec98796cc19" />

<img width="2232" height="934" alt="image" src="https://github.com/user-attachments/assets/a114aaba-6269-4b33-a7fc-d0f9b508b3d9" />

<img width="976" height="807" alt="image" src="https://github.com/user-attachments/assets/fb2545f1-8d54-47eb-af05-3fd41d18d8b7" />

<img width="700" height="437" alt="image" src="https://github.com/user-attachments/assets/7feb221f-3c38-4e1d-b4cb-8046a1c27cc5" />

<img width="720" height="372" alt="image" src="https://github.com/user-attachments/assets/e0612d8d-4df5-4c21-9cb1-ccabe3562d29" />


Step 12 — Ping a Public Website

What you are doing:
Now compare traffic generated by communicating with another computer on the Azure VNet to traffic destined for the public Internet.

From PowerShell:

ping www.google.com



Observe Wireshark.

You should see ICMP packets if the destination responds to ping.

Important Observation

A website does not have to respond to ICMP ping. Therefore, if Google does not return replies, that does not necessarily mean your Internet connection is broken.

You can also test:

ping 8.8.8.8

Observe the difference between:

Internal Azure traffic
Internet-bound traffic
Part 3 — Configure a Firewall / Network Security Group
Step 13 — Use an NSG to Block ICMP

What you are doing:
You will intentionally create an Azure Network Security Group rule that blocks inbound ICMP traffic to the Ubuntu VM. This demonstrates how a firewall can change what traffic is allowed through the network.

Azure NSGs use security rules to control inbound and outbound network traffic.

13.1 Start a Continuous Ping

On the Windows VM:

ping <Ubuntu-private-IP> -t

For example:

ping 10.0.0.5 -t

You should see continuous replies.

13.2 Open the Ubuntu VM's NSG

In Azure:

Open linux-vm.
Select Networking.
Locate the Network security group.
Open the NSG.

Select:

Inbound security rules


13.3 Create a Deny ICMP Rule

Create a new inbound security rule.

Use approximately:

Setting	Value
Source -	Any

Source - port	*

Destination -	Any

Destination - port	*

Protocol -	ICMP

Action -	Deny

Priority -	100

Name -	Deny-ICMP

<img width="551" height="581" alt="image" src="https://github.com/user-attachments/assets/5660fd48-d498-409a-9958-80afc1365fa1" />

<img width="2079" height="1151" alt="image" src="https://github.com/user-attachments/assets/a9899b8c-c1f0-4e4b-9a0a-12ca835ed8bc" />

<img width="883" height="725" alt="image" src="https://github.com/user-attachments/assets/2ec54eab-7df0-45b3-aab6-3f2a5004cd18" />

13.4 Observe the Ping

Return to the Windows VM.

Your command:

ping <Ubuntu-private-IP> -t

should now begin showing:

Request timed out.

At the same time, Wireshark should still show the outgoing ICMP Echo Requests.

Important Concept

The Windows computer can still send the packet.

The NSG is preventing the packet from successfully reaching the Ubuntu VM.

13.5 Re-enable ICMP

Return to the Ubuntu VM's NSG.

Delete or disable the Deny-ICMP rule.

Wait several seconds.

Return to Windows.

The ping should begin working again:

Reply from 10.x.x.x
Reply from 10.x.x.x
Reply from 10.x.x.x
13.6 Stop the Ping

Press:

Ctrl + C

This stops the continuous ping.

Observe SSH Traffic
Step 14 — Log Back Into Windows VM

What you are doing:
Reconnect to the Windows VM using Remote Desktop so that all subsequent packet captures occur from the Windows machine.

Step 15 — Start a Wireshark Capture

Open Wireshark and begin a new packet capture.

Make sure the correct network interface is selected.

Step 16 — Filter for SSH

In the Wireshark filter bar, enter:

ssh

Alternatively, you can use:

tcp.port == 22

The second filter is useful because SSH normally operates over TCP port 22.

Step 17 — SSH Into the Ubuntu VM

What you are doing:
The Windows VM will act as the SSH client while Ubuntu acts as the SSH server.

Open PowerShell on Windows.

Enter:

ssh labuser@<private-IP-address>

For example:

ssh labuser@10.0.0.5

Microsoft documents the standard SSH format as ssh username@IP-address.

Step 18 — Generate SSH Traffic

After connecting to Ubuntu, run several commands:

whoami
pwd
ls
hostname
ip addr

Watch Wireshark while you type.

What should you notice?

You should see TCP traffic involving port 22.

The actual contents of the SSH session are encrypted, so Wireshark can show the network communication but should not simply display your SSH commands as readable plaintext.

Exit SSH

Type:

exit

<img width="800" height="419" alt="image" src="https://github.com/user-attachments/assets/a3e90658-0752-4fe7-81d1-0e49c6080a30" />

<img width="1400" height="577" alt="image" src="https://github.com/user-attachments/assets/3dbb46ab-bfb0-46f9-815f-01eda58ebdde" />

<img width="1650" height="856" alt="image" src="https://github.com/user-attachments/assets/051d0852-e21f-4c88-9f67-7df28ef318af" />

<img width="1716" height="1043" alt="image" src="https://github.com/user-attachments/assets/997d4282-b543-4c5a-8fde-289b8677be10" />




Observe DHCP Traffic
Step 19 — Filter for DHCP

In Wireshark, enter:

dhcp

Depending on the Wireshark version and capture, DHCP may also appear under the BOOTP protocol name.

Step 20 — Attempt to Renew the Windows IP Address

Open PowerShell as Administrator.

Run:

ipconfig /renew

You can also inspect the current configuration with:

ipconfig /all
Important Azure Note

Azure manages VM networking differently from a traditional physical LAN. Therefore, an Azure VM may not produce the same DHCP traffic you would expect on a classroom physical network.

<img width="1136" height="584" alt="image" src="https://github.com/user-attachments/assets/5ebf3510-4ed3-422c-ba6e-8bd7f8ad3af8" />

<img width="772" height="593" alt="image" src="https://github.com/user-attachments/assets/f573a1c1-9fba-49ca-a7e3-22b384667058" />

<img width="914" height="691" alt="image" src="https://github.com/user-attachments/assets/c5637d68-1e28-4ac2-98db-84ca7e465800" />

<img width="1033" height="792" alt="image" src="https://github.com/user-attachments/assets/cfb2c266-2d49-4839-acf9-c1ebb7c9a2b2" />

<img width="966" height="686" alt="image" src="https://github.com/user-attachments/assets/51fd683d-3288-42ff-b0e6-3a5b194961d7" />


Observe DNS Traffic
Step 21 — Filter for DNS

In Wireshark, enter:

dns

This will display DNS queries and responses.

Step 22 — Use NSLookup

Open PowerShell.

Run:

nslookup google.com

Then:

nslookup disney.com

Wireshark should display DNS traffic associated with these lookups.

Conceptually:

Windows VM
     |
     | DNS Query:
     | "What is the IP for google.com?"
     ↓
 DNS Server
     |
     | DNS Response:
     | "google.com = x.x.x.x"
     ↓
Windows VM

<img width="802" height="603" alt="image" src="https://github.com/user-attachments/assets/4987d980-23f4-4206-8c31-1ca51d8cc9c8" />

<img width="1410" height="656" alt="image" src="https://github.com/user-attachments/assets/59dc7608-0e7c-42d2-95dd-c689132d1798" />

<img width="1366" height="728" alt="image" src="https://github.com/user-attachments/assets/d12d2335-688c-49db-a7a8-e968befcf241" />

<img width="1400" height="750" alt="image" src="https://github.com/user-attachments/assets/4d7f2a4c-6560-4f49-93a2-f802d5d6a6e9" />

<img width="784" height="664" alt="image" src="https://github.com/user-attachments/assets/2c7b2e9a-7be2-4ea5-b6fb-85f454c5a9f8" />


Observe RDP Traffic
Step 23 — Filter for RDP

Because you are currently connected to the Windows VM using Remote Desktop, Wireshark should already be capturing RDP-related traffic.

Enter:

tcp.port == 3389

This filters for TCP traffic associated with RDP's standard port.

Step 24 — Observe the Continuous RDP Traffic

You should notice that traffic continues to appear even when you aren't deliberately typing commands or opening applications.

Why?

Remote Desktop maintains an active communication session between the client and Windows VM.

The RDP connection continuously exchanges information needed to maintain the remote desktop session, including screen updates, input information, session management, and other protocol traffic.

Therefore, you shouldn't expect RDP traffic to appear only when you click something.

Student Question

Why does RDP generate traffic continuously instead of only when you perform an action?

Answer:
RDP maintains an active, interactive connection between the local computer and the remote Windows computer. The protocol continuously exchanges information to keep the remote session synchronized and responsive.

Lab Cleanup
Step 25 — Close Remote Desktop

Disconnect from the Windows VM.

On the Remote Desktop client, close the connection.

Do not delete individual resources yet.

Step 26 — Delete the Resource Group

What you are doing:
Deleting the Resource Group will remove the lab resources contained inside it, including the VMs and associated networking resources.

In Azure:

Search for Resource groups.

Open:

network-lab-rg

Select Delete resource group.
Enter the Resource Group name when prompted.
Select Delete.


<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1a2722c9-b95b-4dab-a10b-26dc0f45cc69" />

<img width="882" height="848" alt="image" src="https://github.com/user-attachments/assets/9746de8a-73cf-4c97-80ac-b76f7973acc4" />

<img width="800" height="363" alt="image" src="https://github.com/user-attachments/assets/32370ecb-162b-4b1e-806f-614f649a43d0" />

<img width="800" height="724" alt="image" src="https://github.com/user-attachments/assets/8b7929e5-d8b0-4bf8-9705-0d0823c00517" />


Step 27 — Verify Resource Group Deletion

Return to:

Azure Portal → Resource groups

Refresh the page.

Verify that:

network-lab-rg

no longer appears.

Also check All resources to make sure the lab VMs and related resources have been removed.

⚠️ Final Check

Before leaving the lab, verify:

 Windows VM deleted
 Ubuntu VM deleted
 Virtual Network deleted
 Network Security Groups deleted
 Public IP addresses deleted
 Network interfaces deleted
 Resource Group deleted

Deleting the Resource Group is a convenient way to remove the resources created for the lab together.


Suggested Student Lab Questions
Question 1

What is the purpose of an Azure Resource Group?

Answer: A Resource Group organizes related Azure resources so they can be managed as a group.

Question 2

Why must the Windows and Ubuntu VMs use the same VNet and subnet?

Answer: The lab is designed to allow the VMs to communicate directly over Azure's private network so that their traffic can be observed.

Question 3

What protocol does ping use?

Answer: ICMP.

Question 4

What happens to the ICMP Echo Request when the NSG blocks inbound ICMP?

Answer: The Windows VM can still generate the request, but the NSG prevents the request from successfully reaching the Ubuntu VM.

Question 5

What port does SSH normally use?

Answer: TCP port 22.

Question 6

What port does RDP normally use?

Answer: TCP port 3389.

Question 7

What protocol is used to translate names such as google.com into IP addresses?

Answer: DNS.

Question 8

Why might ipconfig /renew fail to produce obvious DHCP packets in Azure?

Answer: Azure VM networking is virtualized and managed by Azure, so its DHCP behavior isn't identical to a traditional physical LAN.

Question 9

Why does RDP generate traffic continuously?

Answer: RDP maintains an active interactive session and continuously exchanges information needed to keep the remote desktop synchronized.








  

