# Azure Network Security with Wireshark Traffic Analysis

## Project Overview
This project demonstrates network security concepts in Azure using Wireshark to analyze different types of network traffic between virtual machines. It includes configuring network security groups (NSGs) and observing how they affect communication between resources.

## Environments and Technologies Used
- Microsoft Azure (Virtual Machines, Virtual Networks)
- Windows 10 (21H2)
- Ubuntu Server 20.04
- WireShark (Network Protocol Analyzer)
- Various Network Protocols: ICMP, SSH, RDP, DHCP, DNS

## Prerequisites
- Azure subscription
- Understanding of basic networking concepts
- Remote Desktop capability

## Implementation Steps

### Part 1: Creating Azure Resources

#### 1. Resource Group Creation
- Created a new Resource Group in Azure to contain all project resources

![Resource Group Creation](https://i.imgur.com/4FXy9Sa.png)

#### 2. Windows VM Creation
- Deployed a Windows 10 Virtual Machine
- Selected the previously created Resource Group
- Configured VM to create a new Virtual Network (VNet) and Subnet
- Ensured RDP access was enabled (port 3389)

![Windows VM Creation](https://i.imgur.com/jHLqGu3.png)

#### 3. Linux VM Creation
- Deployed an Ubuntu Linux Virtual Machine
- Selected the same Resource Group as the Windows VM
- Chose the same Virtual Network to ensure VM-to-VM communication
- Set authentication type to Username/Password for easier SSH access

![Ubuntu VM Creation](https://i.imgur.com/QF7klzK.png)

#### 4. Network Verification
- Verified both VMs were properly connected to the same VNet/Subnet
- Noted the private IP addresses of both machines for later use

![Network Topology](https://i.imgur.com/nNwsX3z.png)

### Part 2: Analyzing ICMP Traffic

#### 1. Remote Desktop Connection
- Connected to the Windows 10 VM using Remote Desktop
- Installed WireShark within the Windows VM

![Remote Desktop](https://i.imgur.com/KwNc1rL.png)

#### 2. Wireshark Configuration
- Opened Wireshark and started packet capture
- Applied a display filter for ICMP traffic only: `icmp`

![Wireshark ICMP Filter](https://i.imgur.com/NfXdlSm.png)

#### 3. ICMP Testing - Internal Network
- Retrieved the private IP address of the Ubuntu VM
- Opened Command Prompt in the Windows VM
- Pinged the Ubuntu VM: `ping <ubuntu-vm-private-ip>`
- Observed the ICMP request and reply packets in Wireshark

![ICMP Internal Ping](https://i.imgur.com/2EoLMbP.png)

#### 4. ICMP Testing - External Network
- Pinged a public website (www.google.com) from the Windows VM
- Observed the ICMP traffic in Wireshark

![ICMP External Ping](https://i.imgur.com/aGZRDk5.png)

### Part 3: Network Security Group Configuration

#### 1. Continuous Ping Setup
- Initiated a perpetual ping from Windows VM to Ubuntu VM:
- Confirmed ICMP traffic was flowing in Wireshark

![Continuous Ping](https://i.imgur.com/G2XsE0R.png)

#### 2. Firewall Rule Implementation
- Located the Network Security Group (NSG) attached to the Ubuntu VM
- Created a new inbound security rule to block ICMP traffic:
- Priority: 300
- Name: DENY_ICMP
- Protocol: ICMP
- Action: Deny

![NSG Rule Creation](https://i.imgur.com/zCEJV7G.png)

#### 3. Testing Firewall Impact
- Observed the Windows VM command prompt
- Verified ping requests began to time out after implementing the rule
- Examined Wireshark to confirm ICMP requests were sent but no replies were received

![ICMP Blocked](https://i.imgur.com/SdH45Fz.png)

#### 4. Restoring Connectivity
- Re-enabled ICMP traffic by:
- Deleting the deny rule, or
- Creating a higher priority allow rule, or
- Modifying the existing rule to "Allow"
- Confirmed ping requests resumed successfully
- Stopped the continuous ping with Ctrl+C

![ICMP Restored](https://i.imgur.com/QY8f2Je.png)

### Part 4: Analyzing SSH Traffic

#### 1. Wireshark Filter Configuration
- Updated Wireshark to filter for SSH traffic: `ssh or port 22`

![SSH Filter](https://i.imgur.com/vL231Rj.png)

#### 2. SSH Connection
- Opened PowerShell in the Windows VM
- Connected to the Ubuntu VM via SSH:
- Entered the password when prompted
- Executed basic Linux commands: `ls`, `pwd`, `whoami`
- Observed the SSH packets in Wireshark

![SSH Connection](https://i.imgur.com/Ke4mJHv.png)

#### 3. SSH Session Analysis
- Noted how each command generated SSH traffic
- Observed the encrypted nature of SSH packets
- Exited the SSH session with the `exit` command

### Part 5: Analyzing DHCP Traffic

#### 1. Wireshark Filter Setup
- Changed Wireshark to filter for DHCP traffic: `dhcp`

![DHCP Filter](https://i.imgur.com/f2QvJqv.png)

#### 2. DHCP Lease Renewal
- Opened PowerShell as Administrator in the Windows VM
- Forced a DHCP lease renewal:
- Observed the DHCP request and acknowledgment packets in Wireshark

![DHCP Renewal](https://i.imgur.com/5nHrqXk.png)

### Part 6: Analyzing DNS Traffic

#### 1. Wireshark Filter Configuration
- Updated Wireshark to filter for DNS traffic: `dns`

![DNS Filter](https://i.imgur.com/l8JvP6W.png)

#### 2. DNS Query Execution
- Used nslookup to resolve domain names:
- Observed DNS queries and responses in Wireshark
- Analyzed the DNS packet contents including questions and answers

![DNS Queries](https://i.imgur.com/h2eWnG6.png)

### Part 7: Analyzing RDP Traffic

#### 1. Wireshark Filter Setup
- Changed Wireshark to filter for RDP traffic: `tcp.port == 3389`

![RDP Filter](https://i.imgur.com/mJrEbPx.png)

#### 2. RDP Traffic Observation
- Observed the continuous stream of RDP packets
- Noted the consistent traffic even when not actively performing actions
- Discussed why RDP generates constant traffic (continuous screen updates and session maintenance)

![RDP Traffic](https://i.imgur.com/Pj2EL7i.png)

### Part 8: Project Cleanup

#### 1. Resource Removal
- Closed the Remote Desktop connection
- Returned to the Azure Portal
- Deleted the Resource Group containing the VMs
- Verified all resources were properly removed

![Resource Cleanup](https://i.imgur.com/YX21Qxa.png)

## Lessons Learned
- Practical understanding of different network protocols and their traffic patterns
- How Network Security Groups function as firewalls in Azure
- The impact of security rules on network communication
- Using Wireshark for network traffic analysis and troubleshooting
- The importance of cleaning up cloud resources to prevent unnecessary charges
