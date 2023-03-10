# CompanyNetwork
![Topologia-Company1](https://user-images.githubusercontent.com/123495174/224367967-aa4eb05a-1f56-4a22-b47e-6d0745e38bea.png)
### Company Network 


## 
This network is Building with 3 floors with 2 department in each floor.



---
# Description of proyect:

Things we use to build this network:
-	Cisco Packertracer
-	Hierarchical model to provide redundancy
-	ISP to provide redundancy
-	Wireless network to each department.
-	Different Vlans for each department
-	VLSM Subnetting network
-	We use a PAT translation to connect to a public internet providers
-	Configuration of basic setting (hostname, console password, enable password, banner messages, disable ip domain loop up)
-	Communication between Vlans inter-Vlan Routing with a Multilayers Switch
-	The multilayers Switches have Routing and Switching functionalities
-	DHCP server connection.
-	Devices in the server room are to be allocated IP address statically
-	We use OSPF as routing protocol 
-	SSH to remote login 
-	Port-security for Finance and Accounts department. Sticky method to obtain mac-address and violation shut down.
-	PAT  to use the respective outbound roouter interface ipv4 address. With its necessary ACL Rules.
-	**Test Communications.**


## VLSM Subnetting NetworkNetworking 
![subneteop1](https://user-images.githubusercontent.com/123495174/224370391-16eb717b-e842-4761-b3c2-8d71735a1db3.png)
![subneteop2](https://user-images.githubusercontent.com/123495174/224370395-e85aa2b6-df04-4ae6-9218-754fafb4bd78.png)
![Subneteop3](https://user-images.githubusercontent.com/123495174/224370397-2faa19c1-0f6b-4022-990b-590701fca1ad.png)

## Devices configuration 

### Sales Switch:
    enable
    conf t
    hostname SALES-SW
    Banner motd #NO Unauthorised Access!!!#
    no ip domain lookup
    line console 0
    password cisco
    login
    exit
    enable password cisco
    service password-encryption
    exit
    wr
    
     // vlan config
    
    int range fa0/1-2
    switchport mode trunk
    exit
    vlan 10
    name SALES
    exit
    int range fa0/3-24
    switchport mode access 
    switchport access vlan 10
    exit
    vlan 99
    name BlackHole
    exit
    int range gig0/1-2
    switchport mode access
    switchport access vlan 99
    shutdown
    exit 
    do wr

### HR Switch configurations 
    enable
    conf t
    hostname HR-SW
    Banner motd #NO Unauthorised Access!!!#
    no ip domain lookup
    line console 0
    password cisco
    login
    exit
    enable password cisco
    service password-encryption
    exit
    wr
    
    ### vlan config  ##
    
    int range fa0/1-2
    switchport mode trunk
    exit
    vlan 20
    name HR
    exit
    int range fa0/3-24
    switchport mode access 
    switchport access vlan 20
    exit
    vlan 99
    name BlackHole
    exit
    int range gig0/1-2
    switchport mode access
    switchport access vlan 99
    shutdown
    exit 
    do wr

### Finance Switch Configuration:
    enable
    conf t
    hostname Finance-SW
    Banner motd #NO Unauthorised Access!!!#
    no ip domain lookup
    line console 0
    password cisco
    login
    exit
    enable password cisco
    service password-encryption
    exit
    wr
    
    ## Vlan config##
    
    int range fa0/1-2
    switchport mode trunk
    exit
    vlan 30
    name FINANCE
    exit
    int range fa0/3-24
    switchport mode access 
    switchport access vlan 30
    exit
    vlan 99
    name BlackHole
    exit
    int range gig0/1-2
    switchport mode access
    switchport access vlan 99
    shutdown
    exit 
    do wr
    
    ##Port Security maximo 1 usuario por interface y con unica mac-address ##
    
    int range fa0/3-24
    switchport port-security maximum 1
    switchport port-security mac-address sticky
    switchport port-security violation shutdown
    exit
    do wr


### Admin Switch config:
    enable
    conf t
    hostname Admin-SW
    Banner motd #NO Unauthorised Access!!!#
    no ip domain lookup
    line console 0
    password cisco
    login
    exit
    enable password cisco
    service password-encryption
    exit
    wr
    ---
    ## Vlan config  ##
    ---
    int range fa0/1-2
    switchport mode trunk
    exit
    vlan 40
    name ADMIN
    exit
    int range fa0/3-24
    switchport mode access 
    switchport access vlan 40
    exit
    vlan 99
    name BlackHole
    exit
    int range gig0/1-2
    switchport mode access
    switchport access vlan 99
    shutdown
    exit 
    do wr
    
### ITC Switch Configuration:
    enable
    conf t
    hostname -SW
    Banner motd #NO Unauthorised Access!!!#
    no ip domain lookup
    line console 0
    password cisco
    login
    exit
    enable password cisco
    service password-encryption
    exit
    wr
    
    ## vlan config  ##
    
    int range fa0/1-2
    switchport mode trunk
    exit
    vlan 50
    name ITC
    exit
    int range fa0/3-24
    switchport mode access 
    switchport access vlan 50
    exit
    vlan 99
    name BlackHole
    exit
    int range gig0/1-2
    switchport mode access
    switchport access vlan 99
    shutdown
    exit 
    do wr


###  Server Rooom Switch Configuration:
    enable
    conf t
    hostname ServerRoom-SW
    Banner motd #NO Unauthorised Access!!!#
    no ip domain lookup
    line console 0
    password cisco
    login
    exit
    enable password cisco
    service password-encryption
    exit
    wr
    
    ## vlan config
    
    int range fa0/1-2
    switchport mode trunk
    exit
    vlan 60
    name ServerRoom
    exit
    int range fa0/3-24
    switchport mode access 
    switchport access vlan 60
    exit
    vlan 99
    name BlackHole
    exit
    int range gig0/1-2
    switchport mode access
    switchport access vlan 99
    shutdown
    exit 
    do wr
   

### Multilayer Switch  1 Mlt-SW1 Configuration:
    enable
    conf t
    hostname Mlt-SW1
    Banner motd #NO Unauthorised Access!!!#
    no ip domain lookup
    line console 0
    password cisco
    login
    exit
    enable password cisco
    service password-encryption
    
    ip domain-name cisco.net
    username admin password cisco
    crypto key generate rsa
    1024
    line vty 0 15
    login local 
    transport input ssh
    exit
    ip ssh version 2
    do wr
    
    ##Interfaces mode trunk  Mlt sw1
    
    int range gig1/0/3-8
    switchport mode trunk 
    exit
    vlan 10
    name SALES
    vlan 20
    name HR
    vlan 30
    name FINANCE
    vlan 40
    name ADMIN
    vlan 50
    name ITC
    vlan 60
    name ServerRoom
    exit
    do wr
    
    ##Ip configuration Mlt-SW1
    
    ip routing
    
    interface gig1/0/1
    no switchport 
    ip address 172.16.3.145 255.255.255.252
    no shutdown
    exit do wr
    
    interface gig1/0/2
    no switchport 
    ip address 172.16.3.149 255.255.255.252
    no shutdown
    exit
    do wr
    
    ## OSPF Configuration 
    
    router ospf 10
    router-id 2.2.2.2
    network 172.16.1.0 0.0.0.127 area 0
    network 172.16.1.129 0.0.0.127 area 0
    network 172.16.2.0 0.0.0.127 area 0
    network 172.16.2.128 0.0.0.127 area 0
    network 172.16.3.0 0.0.0.127 area 0
    network 172.16.3.128 0.0.0.15 area 0
    network 172.16.3.144 0.0.0.3 area 0
    network 172.16.3.148 0.0.0.3 area 0
    exit
    do wr
    
    ## inter vlan routing and helper-address
    
    int vlan 10
    no shutdown
    ip address 172.16.1.1 255.255.255.128
    ip helper-address 172.16.3.130
    
    int vlan 20
    no shutdown
    ip address 172.16.1.129 255.255.255.128
    ip helper-address 172.16.3.130
    
    int vlan 30
    no shutdown
    ip address 172.16.2.1 255.255.255.128
    ip helper-address 172.16.3.130
    
    int vlan 40
    no shutdown
    ip address 172.16.2.129 255.255.255.128
    ip helper-address 172.16.3.130
    
    int vlan 50
    no shutdown
    ip address 172.16.3.1 255.255.255.128
    ip helper-address 172.16.3.130
    
    int vlan 60
    no shutdown
    ip address 172.16.3.129 255.255.255.240
    ip helper-address 172.16.3.130
    
    exit 
    do wr 
    
    //  Default route  
    ip route 0.0.0.0 0.0.0.0 gig1/0/1
    ip route 0.0.0.0 0.0.0.0 gig1/0/2 100
    do wr
    
### Multilayer switch 2 Mlt-SW2  Configuration:
    enable
    conf t
    hostname Mlt-SW2
    Banner motd #NO Unauthorised Access!!!#
    no ip domain lookup
    line console 0
    password cisco
    login
    exit
    enable password cisco
    service password-encryption
    
    ip domain-name cisco.net
    username admin password cisco
    crypto key generate rsa
    1024
    line vty 0 15
    login local 
    transport input ssh
    exit
    ip ssh version 2
    do wr
    
    
    ##nterfaces mode trunk  Mlt sw2
    
    int range gig1/0/3-8
    switchport mode trunk 
    exit
    vlan 10
    name SALES
    vlan 20
    name HR
    vlan 30
    name FINANCE
    vlan 40
    name ADMIN
    vlan 50
    name ITC
    vlan 60
    name ServerRoom
    exit
    do wr
    
    ##ip configuration Mlt-SW2
    
    ip routing
    
    interface gig1/0/1
    no switchport 
    ip address 172.16.3.153 255.255.255.252
    no shutdown
    exit
    
    interface gig1/0/2
    no switchport 
    ip address 172.16.3.157 255.255.255.252
    no shutdown
    
    ## inter vlan routing and helper-address
    
    int vlan 10
    no shutdown
    ip address 172.16.1.1 255.255.255.128
    ip helper-address 172.16.3.130
    
    int vlan 20
    no shutdown
    ip address 172.16.1.129 255.255.255.128
    ip helper-address 172.16.3.130
    
    int vlan 30
    no shutdown
    ip address 172.16.2.1 255.255.255.128
    ip helper-address 172.16.3.130
    
    int vlan 40
    no shutdown
    ip address 172.16.2.129 255.255.255.128
    ip helper-address 172.16.3.130
    
    int vlan 50
    no shutdown
    ip address 172.16.3.1 255.255.255.128
    ip helper-address 172.16.3.130
    
    int vlan 60
    no shutdown
    ip address 172.16.3.129 255.255.255.240
    ip helper-address 172.16.3.130
    
    exit 
    do wr 
    
    
    ## Default route  
    ip route 0.0.0.0 0.0.0.0 gig1/0/1
    ip route 0.0.0.0 0.0.0.0 gig1/0/2 100
    do wr

### Core router 1 Configuration:

    enable
    conf t
    hostname CORE-R1
    Banner motd #NO Unauthorised Access!!!#
    no ip domain lookup
    line console 0
    password cisco
    login
    exit
    enable password cisco
    service password-encryption
    
    ip domain-name cisco.net
    username admin password cisco
    crypto key generate rsa
    1024
    line vty 0 15
    login local 
    transport input ssh
    exit
    ip ssh version 2
    do wr
    
    ##ip configuration CORE-R1
    
    
    interface gig0/0
    ip address 172.16.3.146 255.255.255.252
    no shutdown
    exit
    
    interface gig0/1 
    ip address 172.16.3.154 255.255.255.252
    no shutdown
    exit
    do wr
    
    interface se0/1/0 
    clock rate 64000
    ip address 195.136.17.1 255.255.255.252
    no shutdown
    
    interface se0/1/1
    clock rate 64000
    ip address 195.136.17.5 255.255.255.252
    no shutdown
    exit
    do wr
    
    ##OSPF Confiiguration
    
    router ospf 10
    router-id 3.3.3.3
    network 172.16.3.144 0.0.0.3 area 0
    network 172.16.3.152 0.0.0.3 area 0
    network 195.136.17.0 0.0.0.3 area 0 
    network 195.136.17.4 0.0.0.3 area 0
    exit
    do wr
    
    ## PAT Configuration 
    
    ip nat inside source list 1 int se0/1/0 overload
    ip nat inside source list 1 int se0/1/1 overload
    
    access-list 1 permit 172.16.1.0 0.0.0.127
    access-list 1 permit 172.16.1.128 0.0.0.127
    access-list 1 permit 172.16.2.0 0.0.0.127
    access-list 1 permit 172.16.2.128 0.0.0.127
    access-list 1 permit 172.16.3.0 0.0.0.127
    access-list 1 permit 172.16.3.128 0.0.0.15
    do wr
    
    int range gig0/0-1
    ip nat inside 
    exit
    
    
    int se0/1/0
    ip nat outside
    int se0/1/1
    ip nat outside
    exit
    do wr
    
    ## Default route  
    ip route 0.0.0.0 0.0.0.0 se0/1/0
    ip route 0.0.0.0 0.0.0.0 se0/1/1 100
    do wr
    
### Core router 2 Configuration:
    enable
    conf t
    hostname CORE-R2
    Banner motd #NO Unauthorised Access!!!#
    no ip domain lookup
    line console 0
    password cisco
    login
    exit
    enable password cisco
    service password-encryption
    
    ip domain-name cisco.net
    username admin password cisco
    crypto key generate rsa
    1024
    line vty 0 15
    login local 
    transport input ssh
    exit
    ip ssh version 2
    do wr
    
    
    ##ip configuration CORE-R2
    
    
    interface gig0/0
    ip address 172.16.3.150 255.255.255.252
    no shutdown
    
    interface gig0/1 
    ip address 172.16.3.158 255.255.255.252
    no shutdown
    
    interface se0/1/0 
    clock rate 64000
    ip address 195.136.17.9 255.255.255.252
    no shutdown
    
    interface se0/1/1
    clock rate 64000
    ip address 195.136.17.13 255.255.255.252
    no shutdown
    exit
    do wr
    
    ## OSPF Confiiguration
    
    router ospf 10
    router-id 4.4.4.4
    network 172.16.3.152 0.0.0.3 area 0
    network 172.16.3.156 0.0.0.3 area 0
    network 195.136.17.8 0.0.0.3 area 0 
    network 195.136.17.12 0.0.0.3 area 0
    exit
    do wr
    
    ## PAT Configuration 
    
    ip nat inside source list 1 int se0/1/0 overload
    ip nat inside source list 1 int se0/1/1 overload
    
    access-list 1 permit 172.16.1.0 0.0.0.127
    access-list 1 permit 172.16.1.128 0.0.0.127
    access-list 1 permit 172.16.2.0 0.0.0.127
    access-list 1 permit 172.16.2.128 0.0.0.127
    access-list 1 permit 172.16.3.0 0.0.0.127
    access-list 1 permit 172.16.3.128 0.0.0.15
    
    int range gig0/0-1
    ip nat inside 
    exit
    
    
    int se0/1/0
    ip nat outside
    int se0/1/1
    ip nat outside
    exit
    do wr
    
### ISP-Main Configuration:

    interface se0/3/0
    ip address 195.136.17.2 255.255.255.252
    no shutdown
    
    interface se0/3/1
    ip address 195.136.17.10 255.255.255.252
    no shutdown
    
    ##OSPF Confiiguration
    
    router ospf 10
    router-id 5.5.5.5
    network 195.136.17.0 0.0.0.3 area 0 
    network 195.136.17.8 0.0.0.3 area 0
    exit
    do wr 
### ISP-Backup Configuration:
    interface se0/3/0
    ip address 195.136.17.6 255.255.255.252
    no shutdown
    
    interface se0/3/1
    ip address 195.136.17.14 255.255.255.252
    no shutdown
    
    exit
    do wr
    
    ## OSPF Confiiguration
    
    router ospf 10
    router-id 6.6.6.6
    network 195.136.17.4 0.0.0.3 area 0 
    network 195.136.17.12 0.0.0.3 area 0
    exit
    do wr 

### Access point Configuration 
![ap](https://user-images.githubusercontent.com/123495174/224376465-0cb239c2-4cd0-4f35-a730-c5fd23efb8e9.png)

### DHCP Server Configuration 
![DHCP](https://user-images.githubusercontent.com/123495174/224376470-af79340b-6ea9-40db-a58a-7960e47886ba.png)

### DNS Server Configuration
![DNS](https://user-images.githubusercontent.com/123495174/224376474-6d0cafd8-7799-4659-9f76-71e39c328454.png)

### Test Connection 
![ping1](https://user-images.githubusercontent.com/123495174/224376475-38049e6b-c022-4ed4-bbe1-aa6f9ba80107.png)

### SSH Test Conecction to Multilayer 2.
![SSH Conection](https://user-images.githubusercontent.com/123495174/224376478-f85ad49b-8068-455b-934a-243f784a9249.png)
