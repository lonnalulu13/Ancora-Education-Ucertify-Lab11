# Ancora-Education-Ucertify-Lab11
Configuring a Site-to-Site IPsec VPN Topology

A site-to-site IPsec Virtual Private Network (VPN) allows two different networks to communicate securely over an unsecured network, such as the Internet. This is achieved by creating an encrypted VPN tunnel between routers located at each site. IPsec provides data confidentiality, authentication, and integrity. It encrypts packets before transmitting them across the public network and decrypts them at the receiving end. The Internet Key Exchange (IKE) protocol is used to negotiate security parameters and establish secure communication between VPN peers.

> **Original Lab Source**
> This lab was available thrpugh:
> [https://ancoraeducation.ucertify.com/app/?func=navigate_items&item_sequence=3]

## Objective of the Lab
This lab session demonstrates the steps involved in configuring a site-to-site IPsec VPN topology. Upon completion of this lab, you will be able to:

Access the R8 terminal and configure the IPsec VPN. 

Configure the IPsec VPN on R9.

## Instructions
## PART A: Accessing the R8B Terminal and Configuring the IPsec VPN
### STEP 1
On the left sidebar, click the PuTTY SSH Client icon

### STEP 2
In the PuTTY Configuration dialog box, type Host Name (or IP address) as 192.168.116.128 and replace the existing Port number with 5003

### STEP 3
From the Connection type list, select the Other radio button and click Open to open the R8 terminal window.

**Caution**

If you don't get a command line prompt in the terminal window, press Enter.

### STEP 4
Execute the following commands to enter privileged EXEC mode and then enter global configuration mode (Note: Execute one command at a time.):
```bash
en
```
```bash
conf t
```
### NOTE
en: This command enters privileged EXEC mode. 

conf t: This command enters global configuration mode to allow device configuration.

### STEP 5
Execute the following commands to configure the IKE policy and transform set (Note: Execute one command at a time.):
```bash
crypto isakmp policy 10
```
```bash
hash sha
```
```bash
authentication pre-share
```
```bash
crypto isakmp key superSecretKey! address 192.168.3.2
```
```bash
crypto ipsec transform-set myset esp-aes 256 esp-sha-hmac
```
### NOTE
crypto isakmp policy 10: This command creates IKE policy 10, which is used to negotiate security parameters.

hash sha: This command specifies SHA hashing for data integrity.

authentication pre-share: This command enables pre-shared key authentication between VPN peers. 

crypto isakmp key superSecretKey! address 192.168.3.2: This command defines the pre-shared key used to authenticate the remote VPN peer. 

crypto ipsec transform-set myset esp-aes 256 esp-sha-hmac: This command defines the IPsec transform set specifying encryption (AES-256) and authentication (SHA).

### STEP 6
Execute the following commands to configure the crypto map (Note: Execute one command at a time.):
```bash
crypto map mymap 10 ipsec-isakmp
```
```bash
set peer 192.168.3.2
```
```bash
set transform-set myset
```
```bash
match address 100
```

### NOTE
crypto map mymap 10 ipsec-isakmp: This command creates a crypto map entry used to define VPN parameters. 

set peer 192.168.3.2: This command specifies the remote VPN peer IP address.

set transform-set myset: This command applies the transform set to the crypto map. 

match address 100: This command specifies the access list used to identify interesting traffic for the VPN tunnel.

### STEP 7
Execute the following commands to configure the router interfaces (Note: Execute one command at a time.):
```bash
interface e0/1
```
```bash
ip address 192.168.1.1 255.255.255.0
```
```bash
interface e0/0
```
```bash
ip address 192.168.3.1 255.255.255.0
```

### NOTE
interface e0/1: This command enters configuration mode for Ethernet0/1. 

ip address 192.168.1.1 255.255.255.0: This command assigns an IP address to the interface connected to the internal network.

interface e0/0: This command enters configuration mode for Ethernet0/0. 

ip address 192.168.3.1 255.255.255.0: This command assigns an IP address to the interface connected to the external network.

### STEP 8
Execute the following commands to configure routing and the crypto ACL (Note: Execute one command at a time.):
```bash
crypto map mymap
```
```bash
ip route 0.0.0.0 0.0.0.0 192.168.3.2
```
```bash
access-list 100 permit ip 192.168.1.0 0.0.0.255 192.168.2.0 0.0.0.255
```
```bash
end
```

#### NOTE
crypto map mymap: This command enters crypto map configuration mode.

ip route 0.0.0.0 0.0.0.0 192.168.3.2: This command creates a default route pointing to the remote peer. 

access-list 100 permit ip 192.168.1.0 0.0.0.255 192.168.2.0 0.0.0.255: This command defines the interesting traffic that will be encrypted through the VPN tunnel.

end: This command exits configuration mode.

### STE 9
Minimize the R8 termial window

## PART B: Configuring the IPsec VPN on R9
### STEP 1
On the left sidebar, right-click the PuTTY SSH Client icon and click New Window.

### STEP 2
In the PuTTY Configuration dialog box, type Host Name (or IP address) as 192.168.116.128 and replace the existing Port number with 5004

### STEP 3
From the Connection type list, select the Other radio button and click Open to open the R9 terminal window.

**Caution**

If you don't get a command line prompt in the terminal window, press Enter.

### STEP 4
Execute the following commands to enter privileged EXEC mode and then enter global configuration mode (Note: Execute one command at a time.):
```bash
en
```
```bash
conf t
```

### STEP 5
Execute the following commands to configure the IKE policy and transform set (Note: Execute one command at a time.):
```bash
crypto isakmp policy 10
```
```bash
hash sha
```
```bash
authentication pre-share
```
```bash
crypto isakmp key superSecretKey! address 192.168.3.2
```
```bash
crypto ipsec transform-set myset esp-aes 256 esp-sha-hmac
```

### STEP 6
Execute the following commands to enter privileged EXEC mode and global configuration mode (Note: Execute one command at a time.):
```bash
crypto map mymap 10 ipsec-isakmp
```
```bash
set peer 192.168.3.2
```
```bash
set transform-set myset
```
```bash
match address 100
```

### STEP 7
Execute the following commands to enter privileged EXEC mode and global configuration mode (Note: Execute one command at a time.):
```bash
interface e0/1
```
```bash
ip address 192.168.1.1 255.255.255.0
```
```bash
interface e0/0
```
```bash
ip address 192.168.3.1 255.255.255.0
```

### STEP 8
Execute the following commands to enter privileged EXEC mode and global configuration mode (Note: Execute one command at a time.):
```bash
crypto map mymap
```
```bash
ip route 0.0.0.0 0.0.0.0 192.168.3.2
```
```bash
access-list 100 permit ip 192.168.1.0 0.0.0.255 192.168.2.0 0.0.0.255
end
```

### STEP 9
Execute the following command to verify the crypto map configuration:
```bash
show crypto map
```

### NOTE

show crypto map: This command displays the configured crypto map parameters, including the peer address, transform set, and access list used for VPN traffic.

## LAB SUMMARY
Now, you are equipped with the knowledge and skills to configure a site-to-site IPsec VPN topology.

Submit your task, and after that, you can perform some additional tasks/activities given below: 

>>Modify the IKE policy parameters and observe how the VPN negotiation changes.

>>Add additional networks to the crypto access list to encrypt more traffic.

>>Use the show crypto isakmp sa command to verify the security associations created for the VPN tunnel.

## Disclaimer

This repository is for **educational and personal learning purposes only**.  

The original lab content belongs to **uCertify / Ancora Education** and remains their copyrighted material.  
This repository is not affiliated with, endorsed by, or sponsored by uCertify or Ancora Education.  

No copyright infringement is intended.  
Use of any tools or techniques mentioned here should only be performed in authorized, legal environments.
