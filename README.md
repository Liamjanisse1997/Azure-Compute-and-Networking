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

Sign in to the Azure Portal.
Search for Resource groups.
Select Resource groups.
Click + Create.
Select your Azure subscription.

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

Click Create.




