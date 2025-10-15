# **Windows Network Services Integration**

## **Introduction and Objectives**

This lab connects three essential Windows Server network services: **DHCP**, **HTTP (IIS)**, and **DNS**. Each service is configured step by step to create a working local network where devices receive automatic IP addresses, access a web page, and resolve names through DNS. 

The goal is to understand how these Windows components interact in one complete environment.


![TOPOLOGY-map](images/Pasted%20image%2020251015230020.png)


The objectives are:

- Configure DHCP for automatic IP assignment
    
- Install and set up IIS to host an internal web page
    
- Add DNS for hostname resolution (_server.lab04.local_)
    
- Verify connectivity and name resolution from clients
    



## Lab Structure

1. [DHCP Configuration on Windows-Server](01-dhcp-configuration-on-windows-server.md)
    
2. [HTTP Configuration on Windows-Server](02-http-configuration-on-windows-server.md)
    
3. [DNS Configuration on Windows Server](
    



## Key Features

- Automatic IP addressing with DHCP scope and gateway assignment
    
- Internal HTTP access using IIS web server
    
- DNS Forward Lookup Zone resolving server hostname
    
- DHCP Option 006 distributing DNS automatically
    
- Browser access through http://server.lab04.local confirming full service connectivity
    



## Author’s Note

In this lab, I learned how to set up key Windows functions for the first time. It was a great experience to run three Windows virtual machines and make all three services work together successfully. Seeing DHCP, HTTP, and DNS integrated and communicating properly felt like building a small, realistic IT environment from scratch.

---

© 2025 - **Lukas Dula** | Home Network Lab & Portfolio
