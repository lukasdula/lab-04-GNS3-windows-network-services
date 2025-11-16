
# 3 - **DNS Configuration on Windows Server**

<br><br>

### 3.1 Introduction

The Domain Name System (DNS) service provides name resolution between hostnames and IP addresses. In this configuration, the Windows Server acts as the DNS server for the lab network, allowing clients to reach the web server by name (server.lab04.local) instead of using its IP address.

![TOPOLOGY-map-3](images/Pasted%20image%2020251015224430.png)

<br><br>

## 3.2 Network Topology

| Device         | Interface | Connected to →             | Peer Interface | IP Address                | Subnet Mask   |
| -------------- | --------- | -------------------------- | -------------- | ------------------------- | ------------- |
| Windows-Server | Ethernet0 | SW1                        | Gi0/0 (SW1)    | 192.168.10.10             | 255.255.255.0 |
| WPC1           | Ethernet0 | SW1                        | Gi0/1 (SW1)    | DHCP (192.168.10.100–150) | 255.255.255.0 |
| WPC2<br>       | Ethernet0 | SW1                        | Gi0/2 (SW1)    | DHCP (192.168.10.100–150) | 255.255.255.0 |
| SW1            | –         | Windows-Server, WPC1, WPC2 | –              | –                         | –             |

<br><br>

## 3.3 Steps

1. Install DNS Server Role via Add Roles and Features Wizard in Server Manager.
    
2. Create Forward Lookup Zone named _lab04.local_.
    
3. Add New Host (A Record) with hostname _server_ and IP _192.168.10.10_.
    
4. Configure DHCP Option 006 (DNS Server) to distribute _192.168.10.10_ to clients.
    
5. Confirm that record _server.lab04.local → 192.168.10.10_ exists in DNS Manager.
    
6. Verify that DNS service is running on Windows Server and that clients receive DNS configuration from DHCP and resolve the server name.

<br><br>

## 3.4 Configuration (Windows Server)

### DNS Role Installation

The DNS Server role is installed using the Add Roles and Features Wizard. After completing the installation, the role appears in Server Manager and can be managed through DNS Manager.

![DNS-installation](images/Pasted%20image%2020251015210349.png)

---

### Forward Lookup Zone Creation

A Forward Lookup Zone named _lab04.local_ is created to provide hostname-to-IP resolution within the internal network.

![lookup-zone](images/Pasted%20image%2020251015213623.png)

---

### Adding Host Record

A new Host (A) record named _server.lab04.local_ is created and mapped to IP address 192.168.10.10.

![adding-host](images/Pasted%20image%2020251015213857.png)

---

### DHCP Integration

The DHCP server is configured to automatically assign the DNS server address (192.168.10.10) to all clients through Scope Option 006.

![DHCP-integration](images/Pasted%20image%2020251015215337.png)

<br><br>

## 3.5 Verification


### Windows-Server

```
Get-Service DNS
```
![verify-dns-1](images/Pasted%20image%2020251015222637.png)

#### **Result**

The DNS Server service is running, confirming that the role is active and ready to respond to DNS queries.

---

### WPC1 (Command Prompt)


```
nslookup server.lab04.local
ping server.lab04.local  
```
![verify-dns-2](images/Pasted%20image%2020251015222236.png)


#### **Result**

DNS resolution and ICMP response received from 192.168.10.10, confirming successful name-to-IP translation.

---


### WPC2 (Command Prompt)


```
nslookup server.lab04.local
ping server.lab04.local  
```
![veirfy-dns-3](images/Pasted%20image%2020251015222111.png)

#### **Result**

DNS resolution and ICMP response received from 192.168.10.10, confirming successful name-to-IP translation.

---

### Browser Verification (WPC2 - Client)

In a web browser, enter:  

```
http://server.lab04.local
```
![verify-dns-4](images/Pasted%20image%2020251015222322.png)


#### **Result**

The IIS default page loads successfully, confirming that DNS and HTTP integration are fully operational.

<br><br>

## 3.6 Conclusion

The DNS configuration on Windows Server successfully translates hostnames to IP addresses within the lab network. Clients receive the DNS configuration from DHCP and can access the IIS web server using the domain name _server.lab04.local_. DNS, DHCP, and HTTP integration operate correctly across all devices.

---

**Back to lab overview:** [README](README.md)

