# Working Steps

1. Create VPC
1. Create public and private subnets
1. Create Internet gateway
1. Attach IGW to VPC
1. Create public and private route tables
1. Associate route tables with subnets
1. Add IGW to public route table
1. Create Elastic IP
1. Associate Elastic IP with VPN instance 
1. Convert ssh pem key to ppk
1. Log in via Putty
    1. `openvpnas@(publicip)`
* OpenVPN setup wizard
* OpenVPN setup wizard starts on first login
* Accept EULA: `yes`
* Will this be primary access server node (no for backup or standby node): `yes`
* Specify network interface: (select private ip)
* Accept default port 943 for Admin Web UI
* Accept default port 443 for OpenVPN Daemon
* Should client traffic be routed by default through the VPN?: `no`
    * I think this is `no` for split tunnel
* Should client DNS traffic be routed by default through the VPN? `no`
    * I think we want `no` here as all our dns is public
* Use local authentication via internal DB? `yes`
    * We can move this to AWS AD if need be
* Should private subnets be accessible to clients by default? `yes`
* Do you wish to login to the Admin UI as "openvpn"? `yes`

* Set password
* `sudo passwd openvpn`
* note that we are setting the "openvpn" appliance user password here
* although we are logged in as the "openvpnas" linux user

* Log into Admin Web UI: 
https://(publicip):943/admin
    * username: openvpn
* Accept EULA
* Configuration -> Server Network Settings
    * Verify Hostname is the ElasticIP address
    * Verify Interface and IP Address is Private IP
    * Verify Protocol is 'Both (Multi-daemon mode)'

* Download OpenVPN Client
* https://(publicip):943
    * username: openvpn
    * password: (set above)
    * dropdown: connect
    * Click download link
* Install client
* Connect
    * Right click on taskbar icon
    * Click public IP of VPN, then connect
    * Login
    * Verify private IP assigned from VPN VPC


Security Group: OpenVPN-SG
* Inbound
    * UDP : 1194 : myip
    * SSH : 22 : myip
    * TCP : 943 : myip
    * TCP : 443 : myip
* Outbound
    * All traffic: 0.0.0.0/0

Security Group: PrivateVPN-SG
* Inbound
    * All traffic: (vpc CIDR)
* Outbound
    * All traffic: 0.0.0.0/0

Route Table: openvpc-public
    * local
    * IGW

Route Table: openvpc-private
    * local

    
# Old Walkthru

Creating an OpenVPN Instance for Client Connections 
https://linuxacademy.com/cp/courses/lesson/course/193/lesson/3 

Lecture: Troubleshooting OpenSwan and OpenVPN 
https://linuxacademy.com/cp/courses/lesson/course/193/lesson/9 

Course that deals with real world scenario of connecting networks between data centers: 
https://linuxacademy.com/cp/modules/view/id/23 


* Source: https://linuxacademy.com/cp/courses/lesson/course/193/lesson/3
* OpenVPN Doc: https://docs.openvpn.net/getting-started/amazon-web-services-ec2-tiered-appliance-quick-start-guide/

* VPC: 172.16.0.0/16
* Public Subnet: 172.16.2.0/24
    * IGW attached
* Private Subnet: 172.16.3.0/24

* AWS Marketplace: OpenVPN Access Server
    * 5 devices
        * us-east-1: ami-5e73b923
        * us-east-2: ami-cde8f721
        * us-west-1: ami-d32f38b3
        * us-west-2: ami-97d448ef
    * 10 devices
        * us-east-1: ami-f575bf88
        * us-east-2: ami-cce8f720
        * us-west-1: ami-442f3824
        * us-west-2: ami-5fd34f27
    * 25 devices
        * us-east-1: ami-397eb444
        * us-east-2: ami-3866575d
        * us-west-1: ami-76213616
        * us-west-2: ami-bad14dc2

* VPN Instance
    * AMI (see above)
    * t2.micro
    * Public Subnet
    * Name: OpenVPN
    * Sec Group (should be autofilled)
        * SSH 22 
        * HTTPS 443
        * TCP 943
        * UDP 1194
* Create new elasticIP, attach to VPN instance
* Connect to VPN instance
    * `ssh key.pem openvpn@(publicip)

* OpenVPN setup wizard
* OpenVPN setup wizard starts on first login
* Accept EULA: `yes`
* Will this be primary access server node (no for backup or standby node): `yes`
* Specify network interface: (select private ip)
* Accept default port 943 for Admin Web UI
* Accept default port 443 for OpenVPN Daemon
* Should client traffic be routed by default through the VPN?: `no`
    * I think this is `no` for split tunnel
* Should client DNS traffic be routed by default through the VPN? `yes`
    * I think we want `yes` here to allow DNS resolution to resources in the VPN network
* Use local authentication via internal DB? `yes`
    * We can move this to AWS AD if need be
* Should private subnets be accessible to clients by default? `yes`
* Do you wish to login to the Admin UI as "openvpn"? `yes`

* Set password
* `sudo passwd openvpn`

* Log into Admin Web UI: 
* https://(publicip):943/admin
    * username: openvpn
    * password: (set above)
* Accept EULA
* Configuration -> Server Network Settings
    * Verify Hostname is the ElasticIP address
    * Verify Interface and IP Address is Private IP
    * Verify Protocol is 'Both (Multi-daemon mode)'

* Download OpenVPN Client
* https://(publicip):943
    * username: openvpn
    * password: (set above)
* Install client
* Connect, verify private IP assigned from VPN VPC
* Open console
* ping the private IP of the VPN instance
    * `ping (privateip)`

