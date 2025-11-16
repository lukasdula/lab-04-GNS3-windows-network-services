# 1 - **DHCP Configuration on Windows-Server**

<br><br>

## **1.1 Introduction**

This section focuses on setting up the DHCP service on Windows-Server in an offline local network. The objective is to automatically assign IP addresses to the connected Windows clients (WPC1 and WPC2) within the subnet 192.168.10.0/24. The Windows-Server operates with a static IP address, while both clients receive their configurations dynamically through DHCP.

![TOPOLOGY-map-1](images/Pasted%20image%2020251010212218.png)

<br><br>

## **1.2 Topology**

| Device         | Interface | Connected to →             | Peer Interface | IP Address                | Subnet Mask   |
| -------------- | --------- | -------------------------- | -------------- | ------------------------- | ------------- |
| Windows-Server | Ethernet0 | SW1                        | Gi0/0 (SW1)    | 192.168.10.10             | 255.255.255.0 |
| WPC1           | Ethernet0 | SW1                        | Gi0/1 (SW1)    | DHCP (192.168.10.100–150) | 255.255.255.0 |
| WPC2<br>       | Ethernet0 | SW1                        | Gi0/2 (SW1)    | DHCP (192.168.10.100–150) | 255.255.255.0 |
| SW1            | –         | Windows-Server, WPC1, WPC2 | –              | –                         | –             |

<br><br>

## **1.3 Steps**

1. Configure a **static IP address** on the Windows Server.
    
2. Install and set up the **DHCP Server role** via Server Manager.
    
3. Create a **new DHCP scope**, define the IP range, and activate the scope.
    
4. Change the **network profile** from _Public_ to _Private_ to allow local communication.
    
5. On **all machines** (server and clients), enable the firewall rule “File and Printer Sharing (Echo Request – ICMPv4-In)
    
6. Verify connectivity between the server and clients using ping and DHCP address assignment.

<br><br>

## **1.4 Configuration (Windows-Server)**

```plaintext
Static IP Address: 192.168.10.10
Subnet Mask: 255.255.255.0
DNS Server: 192.168.10.10
```
![DHCP-configuration](images/Pasted%20image%2020251010212242.png)

```
DHCP Scope Configuration:
Scope Name: Local-Server
IP Range: 192.168.10.100 – 192.168.10.150
Excluded Addresses: 192.168.10.1, 192.168.10.10
Lease Duration: 1 day
DHCP Service Status: Running
```
![DHCP-SCOPE](images/Pasted%20image%2020251010212308.png)

<br><br>

## **1.5 Verification (Diagnostics)**

After completing the DHCP setup, verify service operation and connectivity from all devices using the listed commands.

### Windows-Server (PowerShell)

- **Get-Service DHCPServer** – checks if the DHCP service is running.
    
- **ipconfig /all** – displays full network configuration including assigned IP and DNS.
    
- **ping 192.168.10.100 / 101** – tests communication from the server to both clients.
    
- **arp -a** – shows IP-to-MAC mappings (verifies Layer 2 connectivity).

```plaintext
Get-Service DHCPServer
ipconfig /all
```
![verify-1](images/Pasted%20image%2020251010212345.png)

```
ping 192.168.10.100
ping 192.168.10.101
arp -a
```
![verify-2](images/Pasted%20image%2020251010212431.png)

### WPC1 (Command Prompt)

- **ping 192.168.10.10** – tests connectivity from Client 1 -> Server.
    
- **ping 192.168.10.101** – tests connectivity from Client 1 -> Client 2.

```plaintext
ipconfig /renew
ipconfig /all
```
![verify-3](../Pasted%20image%2020251010212455.png)

```
ping 192.168.10.10
ping 192.168.10.101
arp -a
```
![verify-4](images/Pasted%20image%2020251010212533.png)

### WPC2 (Command Prompt)

- **ping 192.168.10.10** – tests connectivity from Client 2 -> Server.
    
- **ping 192.168.10.100** – tests connectivity from Client 2 -> Client 1.

```plaintext
ipconfig /renew
ipconfig /all
```
![verify-5](images/Pasted%20image%2020251010212602.png)

```
ping 192.168.10.10
ping 192.168.10.100
arp -a
```
![verify-6](images/Pasted%20image%2020251010212632.png)

#### Results

All devices obtained IP addresses from the DHCP server and successfully communicated with each other using ICMP. The DHCP service is active, and ARP tables confirm correct MAC-to-IP mapping.

![server-manager](images/Pasted%20image%2020251010212659.png)

>**Notes.:** To enable ping communication between the Windows Server and both clients, it was necessary to manually enable the **“File and Printer Sharing (Echo Request – ICMPv4-In)”** rules in **Windows Defender Firewall** for all profiles (**Domain**, **Private**, and **Public**).  
Once enabled, ICMP traffic was allowed, and the network devices could successfully respond to ping requests during verification.

<br><br>

## **1.6 Conclusion**

The DHCP configuration on Windows-Server was completed successfully. Both clients received dynamic IP addresses from the defined DHCP scope, confirming Layer 3 connectivity across the subnet 192.168.10.0/24. This setup prepares the environment for further DNS and HTTP (IIS) configuration in the next sections.

---

**Next part:** [HTTP Configuration on Windows-Server](02-http-configuration-on-windows-server.md)

