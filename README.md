# P1 - DNS & DHCP Server Administration

This project covers the installation, configuration, and administration of Domain Name System (BIND) and Dynamic Host Configuration Protocol (DHCP) servers on a CentOS 9 virtual machine running in Oracle VirtualBox. The setup provides dynamic IP management and domain name resolution services for a network within a virtualized environment.

## Table of Contents
- [Project Overview](#project-overview)
- [Requirements](#requirements)
- [Architecture](#architecture)
- [Installation](#installation)
- [Configuration](#configuration)
- [Administration](#administration)
- [Testing and Validation](#testing-and-validation)
- [Troubleshooting](#troubleshooting)


## Project Overview
The project demonstrates how to set up a DNS server using BIND and a DHCP server using ISC-DHCP on a CentOS 9 virtual machine, managed through VirtualBox. The DNS server handles domain name resolution, while the DHCP server dynamically assigns IP addresses to network devices in the virtualized environment.

## Requirements
- **VirtualBox**: To create and manage virtual machines.
- **CentOS 9**: As the operating system for the virtual machine.
- **BIND (Berkeley Internet Name Domain)**: For DNS server configuration.
- **ISC-DHCP Server**: For DHCP service.
- Basic knowledge of Linux, VirtualBox, and networking protocols (DNS, DHCP, TCP/IP).

## Architecture
The architecture involves:
- A CentOS 9 virtual machine hosted in VirtualBox.
- BIND for handling DNS queries and resolving domain names.
- ISC-DHCP server for managing IP leases and network configurations.
- A local virtual network inside VirtualBox to simulate real-world network scenarios.

## Installation
### Step 1: Set Up CentOS VM in VirtualBox
1. Install VirtualBox on your system and create a CentOS 9 virtual machine.
2. Allocate sufficient resources (2GB RAM, 2 CPU cores) for smooth operations.
3. Create a Host-only network adapter in VirtualBox for DHCP and DNS operations.

### Step 2: Install BIND (DNS Server)
```bash
sudo yum install bind bind-utils -y
Step 3: Install ISC-DHCP Server
bash
Copy code
sudo yum install dhcp-server -y
Configuration
Step 1: Configure BIND for DNS
Edit the /etc/named.conf file to define:
Forward and reverse DNS zones.
Access control lists (ACLs).
Recursion settings for internal name resolution.
Example zone configuration in /etc/named/yourdomain.com.zone:
bash
Copy code
zone "yourdomain.com" {
    type master;
    file "/var/named/yourdomain.com.zone";
    allow-query { any; };
};
Step 2: Configure ISC-DHCP for DHCP
Edit /etc/dhcp/dhcpd.conf to:
Define subnets and IP address ranges.
Set default lease times and options like DNS server and gateway.
Example configuration:

bash
Copy code
subnet 192.168.56.0 netmask 255.255.255.0 {
    range 192.168.56.100 192.168.56.200;
    option domain-name-servers 192.168.56.1;
    option routers 192.168.56.1;
    default-lease-time 600;
    max-lease-time 7200;
}
Step 3: Start and Enable Services
bash
Copy code
sudo systemctl enable named
sudo systemctl start named
sudo systemctl enable dhcpd
sudo systemctl start dhcpd
Administration
DNS Server: Use rndc reload to reload DNS zones after editing zone files.
DHCP Server: Monitor lease assignments and update configurations as needed. Ensure the correct IP range is defined.
Logs: Check /var/log/messages and /var/log/named for troubleshooting and monitoring server health.
Testing and Validation
Step 1: Test DNS Queries
On a client machine within the same network, test DNS resolution:

bash
Copy code
dig @<DNS-Server-IP> <domain-name>
Step 2: Test DHCP Lease Assignment
On a client machine (another VM or physical host):

Ensure the client is set to obtain an IP via DHCP.
Run:
bash
Copy code
sudo dhclient -r
sudo dhclient
Step 3: Check Connectivity
Ping the DNS server and resolve domain names to verify proper configuration:

bash
Copy code
ping yourdomain.com
Troubleshooting
DNS Issues: Ensure correct syntax in zone files and use named-checkconf and named-checkzone for validation.
DHCP Issues: Check /var/log/messages for errors and ensure IP ranges do not conflict with the host network.
Firewall: Verify that firewalld or iptables allows traffic on ports 53 (DNS) and 67/68 (DHCP).
