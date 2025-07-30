# NetworkingConcepts
In this lab, we will create two virtual machines: Windows and Ubuntu and explore networking protocols including SSH, RDP, DNS, HTTP, and ICMP.

Create a Resource Group in Azure and name it "NetworkingConcepts".
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/d4892de3-e473-49df-bdd0-8a19d89b6bdf" />

Create a Virtual Machine. Enter "WindowsVM" for the name. Select the region that match your resource group region. Select Windows 10 Pro as the image. Set "labuser" as username and create a password.

Create another Virtual Machine. Configuring the Basic Settings for Ubuntu Server In the Basics tab, set up the following configurations:
Subscription: Select the same subscription used for your Windows VM. 
Resource group: Select the existing resource group that contains your Windows VM. 
Virtual machine name: UbuntuVM
Region: Select the same region as your Windows VM for optimal network performance.  
Image: Ubuntu Server 22.04 LTS
VM size: Select a size with adequate resources (similar to your Windows VM). 
Authentication type: Choose password. Username:labuser, create a password.
Inbound port rules: Allow SSH (port 22)

Networking tab 
Under "Virtual network", select the same virtual network used by your Windows VM. This ensures both VMs can communicate with each other on the same network. 
Verify subnet selection (use the same subnet as your Windows VM). Configure Network Security Group to allow necessary traffic:
Ensure SSH (port 22) is allowed for remote management. Add any additional ports needed for your specific applications.
Review advanced networking settings if you need specific IP configurations.

Click "Review + create". Azure will validate your configuration. Verify all settings are correct, particularly:
Confirm the same resource group as your Windows VM. Confirm the same virtual network as your Windows VM. Verify region is consistent with your Windows VM.
After validation passes, click "Create" to deploy the Ubuntu Server VM. The deployment process will take several minutes to complete.
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/447f7eb3-7b42-4980-a711-08cceab4506b" />

Connect to your Windows 10 VM through Remote Desktop Connection using the public address.
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/5f8272df-d571-4bb7-8a78-2b6caff1f433" />

Now let's download Wireshark. Copy and paste "wireshark.org/download.html" into your VM web browser. Select the Windows Installer (64-bit) option.
Double-click the downloaded file. If prompted by User Account Control, click "Yes". Accept the license agreement. Keep all components selected. Accept default installation location. Allow the installation of WinPcap/Npcap packet capture drivers. After intallation, click "Finish".
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/6a8d98f5-3b4b-4f66-8ce1-6e22c3890e1d" />

Launch Wireshark. Identify your active network connection (usually labeled as Ethernet or Wi-Fi). Double-click the interface or select it and click the blue shark fin icon. Packets will appear.
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/8300df4a-717e-4059-9b06-671efddf7393" />

Let's start an active capture to capture only ICMP packets. At the top of the Wireshark window, in the filter bar type "icmp". Click the blue arrow button or press Enter to apply the filter.
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/5dcfd307-9aa1-4d43-95f9-1a83202fde3d" />

Now, let's send a ping to Ubuntu VM using the private address. Open PowerShell on your Windows 10 VM and ping the address. Observe IMCP traffic in Wireshark.Successful pings confirm network connectivity between your VMs  
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/a2f14426-0914-44e4-b2af-3ef183fbf261" />

Let's start a continuous ping by using "ping [UbuntuVM-Private-IP] -t".
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/584c0d9b-31b1-4184-8c27-34f36c5db367" />

Modify the Ubuntu VM's Network Security Group in the Azure Portal. Navigate to Network Security Groups, select the NSG, and click "Inbound security rules". Add a new rule and set it
Priority: 290. Protocol: ICMPv4. Action: Deny. Destination port ranges: *. Click "Add".
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/c6d8f6ad-193d-4dc0-8063-62bd9921d4ba" />

Now, observe the blocked traffic in PowerShell. You will see "Request timed out" messages appear. Observe the traffic in Wireshark. Echo requests continue to be sent, but no echo replies are being received.
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/05d5fef4-3e66-4242-b0fe-3c1be0b0c5fe" />

Now that we've looked at block traffic, let's remove the rule.
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/f7759fcd-6e0b-4cfa-b12e-084edc1bdd65" />

Verify Restored Connectivity.
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/b85abb6f-da16-4853-be0f-d24ac98c4e4f" />

Stop the ping by pressing Ctrl+C and look at the summary of packets sent, received, and lost.
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/23ab2284-d7bb-414d-8f74-87ab21839921" />

Start a new packet capture in Wireshark to capture SSH traffic. You can enter "ssh" or "tcp.port==22" in the filter bar. 
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/1fe1ad57-3acf-4e19-ae5d-cc734a7ab79f" />

Initiate SSH Connection to Ubuntu VM. Use the SSH command with your Ubuntu VM's private IP, and type yes to continue. Enter your password.
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/435f8b34-2c67-4fdc-bde7-9ff6b57f8881" />

Interact with the SSH Session by entering Linux commands. Watch Wireshark as each command generates bursts of packets. 
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/980db34d-f85b-4bb7-9fee-3536b0424da0" />

Terminate the SSH Session by entering "exit" and press Enter to close the connection. In Wireshark, observe the SSH termination packets.
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/3f9eeee5-4097-4f2c-9e52-8afd727008c4" />

Now, configure Wireshark for DHCP Capture by entering "dhcp" in the filter field. The filter includes BOOTP because DHCP is built on the BOOTP protocol.
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/ada3f5b2-8c63-4c00-a882-7caf5ab9959f" />

Initiate DHCP Renewal Process by typing "ipconfig /renew" in Powershell.
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/f10efbfb-53fa-4414-b965-a52b04da084f" />

Configure Wireshark for DNS Capture by entering "dns" in the filter field.
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/eb71eb87-ef48-4119-9e39-fa9321ddddeb" />

Execute DNS Lookups for Google and Facebook by running "nslookup google.com" and "nslookup facebook.com"
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/e8c84631-5f25-4a7f-975e-260543fb26e5" />
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/b1520852-8349-4fdc-8406-df8aa287924b" />

Configure Wireshark for RDP Filtering by entering "rdp" in the filter field. The display will immediately fill with RDP packet data. These packets represent the ongoing Remote Desktop connection between your local computer and the Windows 10 VM. Note the high volume of traffic—RDP continuously streams screen updates and input data.
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/9a17863f-81e1-499e-b66d-31ccab53dc9d" />

The display will immediately fill with RDP packet data with packets that show connection between your local computer and the Windows 10 VM. 
Examine the packet list to observe consistent flow of packets in both directions, regular packet timing patterns, and varying packet sizes depending on screen activity.
Select individual packets to see TCP headers showing source and destination ports, encrypted payload data, and session information in the connection establishment packets.
Generate more traffice by moving your mouse or typing in the VM. 
