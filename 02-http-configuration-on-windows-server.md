
# 2 - **HTTP Configuration on Windows-Server**

## **2.1 Introduction**  

This section describes the setup of the **HTTP (IIS) service** on Windows-Server in the same offline local network previously used for DHCP. The goal is to host a simple local web service accessible by both Windows clients (WPC1 and WPC2) through their dynamically assigned IP addresses. The Windows-Server acts as a web host with a static IP and preconfigured DHCP functionality.

![TOPOLOGY-map-2](images/Pasted%20image%2020251014215842.png)

## **2.2 Topology**

| Device         | Interface | Connected to →             | Peer Interface | IP Address                | Subnet Mask   |
| -------------- | --------- | -------------------------- | -------------- | ------------------------- | ------------- |
| Windows-Server | Ethernet0 | SW1                        | Gi0/0 (SW1)    | 192.168.10.10             | 255.255.255.0 |
| WPC1           | Ethernet0 | SW1                        | Gi0/1 (SW1)    | DHCP (192.168.10.100–150) | 255.255.255.0 |
| WPC2<br>       | Ethernet0 | SW1                        | Gi0/2 (SW1)    | DHCP (192.168.10.100–150) | 255.255.255.0 |
| SW1            | –         | Windows-Server, WPC1, WPC2 | –              | –                         | –             |




## **2.3 Steps**

1. Ensure DHCP service is active and clients have valid IP addresses.
    
2. Install **IIS (Internet Information Services)** via Server Manager → Add Roles and Features.
    
3. Select **Web Server (IIS)** role and complete the installation wizard.
    
4. Confirm that the **Default Web Site** is created and running in IIS Manager.
    
5. Open **Windows Firewall** and allow inbound rules for **World Wide Web Services (HTTP)**.
    
6. Open a browser on the client and navigate to `http://192.168.10.10` to verify that the default IIS welcome page appears.
    
7. Verify that both clients can access the web page using the server IP:  
    `http://192.168.10.10`
    



## **2.4 Configuration (Windows-Server)**

### **IIS Service Verification**

The IIS (Internet Information Services) role is installed and configured on the Windows Server to enable HTTP functionality.  
During the installation, the **Web Server (IIS)** option is selected in _Server Manager → Add Roles and Features_.  4

The server automatically starts the **World Wide Web Publishing Service (W3SVC)** and creates the **Default Web Site** for local access.



```
IIS Service: Installed and Running  
```
![configuration-http](images/Pasted%20image%2020251014204238.png)

#### **Result**

The IIS service is installed and running.  
The Default Web Site is created automatically and remains active, ready to serve HTTP requests from clients.

---

### **HTTP Web Access Test**

A web browser is opened on a Windows Server and clients, and the address `http://192.168.10.10` is entered.  
This test verifies the accessibility of the web server through the local network and ensures correct HTTP functionality.


```
http://192.168.10.10
```
![http-test](images/Pasted%20image%2020251014210527.png)

#### **Result** 

The default **Internet Information Services (IIS)** welcome page appears, confirming that the HTTP service is available, port 80 is open, and the server successfully responds to client connections.


## **2.5 Verification

### **Server Diagnostics**

#### Windows-Server (PowerShell)

```
Get-Service W3SVC
netstat -ano | findstr ":80"
Get-Website

```
![verify-server](images/Pasted%20image%2020251014211226.png)


#### **Results**

- IIS service (W3SVC) is _Running_
    
- TCP port 80 is _LISTENING_
    
- Default Web Site is _Started_ and bound to `http *:80:`

---

### **Client Diagnostics**

#### WPC1 (Command Prompt)

```
Test-NetConnection 192.168.10.10 -Port 80
ping 192.168.10.10
```
![WPC1](images/Pasted%20image%2020251014212250.png)


---

#### WPC2 (Command Prompt)

```
Test-NetConnection 192.168.10.10 -Port 80
ping 192.168.10.10
```
![WPC2](images/Pasted%20image%2020251014212526.png)


#### **Results**

- Ping successful with 0% packet loss
    
- `TcpTestSucceeded : True` confirming HTTP port accessibility
    
- Web page accessible at `http://192.168.10.10` displaying default IIS interface

### **Explanation**  

Both clients successfully accessed the default IIS web page hosted on the Windows-Server using HTTP over port 80. The IIS service was verified as active, confirming end-to-end network and application layer communication.


---


### 2.6 Conclusion

The HTTP (IIS) configuration on Windows Server **is completed successfully**.
The default IIS web page **is accessible** from both Windows clients using HTTP over port 80, confirming that network connectivity, port accessibility, and web service functionality **are fully operational**.  
This configuration **serves as the foundation** for the following DNS implementation phase.


---

**Next part:**