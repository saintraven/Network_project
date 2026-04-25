## Part 1 ipcalc tool

#### 1.1 Networks and masks
#####
   
     1) ip : 192.167.38.54/13
     network address : 192.160.0.0/13

     2) conversion of the mask 255.255.255.0
     to prefix 255.255.255.0/24
     to binary 11111111.11111111.11111111.00000000
     conversion of the /15
     to normal 255.254.0.0
     to binary 11111111.11111110.00000000.00000000
     conversion of the 11111111.11111111.11111111.11110000
     to normal 255.255.255.240
     to prefix 255.255.255.240/28

     3) min and max host 12.167.38.4 network with masks: 
     /8    min: 12.0.0.1       max: 12.255.255.254
     /16   min: 12.167.0.1     max: 12.167.255.254
     /23   min: 12.167.38.1    max: 12.167.39.254
     /4    min: 0.0.0.1        max: 12.255.255.254
#####

#### 1.2 localhost
#####
   
    194.34.23.100 - no
    127.0.0.2 - yes
    127.1.0.1 - yes
    128.0.0.1 - no 

#####
#### 1.3 Network ranges and segments
#####

    1) ranges:
    10.0.0.45 - private
    134.43.0.2 - public
    192.168.4.2 - private
    172.20.250.4 - private 
    172.0.2.1 - public
    192.172.0.1 - private
    172.68.0.2 - public
    172.16.255.255 - private
    10.10.10.10 - private
    192.169.168.1 - public

    2) possible gateway for 10.10.0.0/18 network:
    10.0.0.1 - no
    10.10.0.2 - yes 
    10.10.10.10 - yes
    10.10.100.1 - no
    10.10.1.255 - yes

#####

- example of work ipcalc:

![ipcalc](../misc/my_images/ipcalc_exmp.png)

- how to define net ip:

![net_definition](../misc/my_images/alg_definition_net.png)

- how to define min and max hosts:

#####

    `MAX` host = 2^n - 2 
    where n = host bits

    `MIN` host = the first available IP + 1 

#####


#### IP Classification
#####

    There are classifications of IP addresses as "private" and "public". The following ranges of addresses are reserved for private (aka LAN) networks:
    - *10.0.0.0* — *10.255.255.255* (*10.0.0.0/8*);
    - *172.16.0.0* — *172.31.255.255* (*172.16.0.0/12*);
    - *192.168.0.0* — *192.168.255.255* (*192.168.0.0/16*);
    - *127.0.0.0* — *127.255.255.255* (Reserved for loopback interfaces (not used for communication between network nodes), so called localhost).

#####
## Part 2 Static routing between two machines

- output of `ip a` for ws1:

![net](../misc/my_images/part2_1.png)

- output of `ip a` for ws2:

![net](../misc/my_images/part2_2.png)

where:

#####

    enp0s3 — name of net interface ;
    link/ether - MAC;
    inet — IP

#####

- updated and applied netplan config for ws1:

![upd_conf](../misc/my_images/part2_3.png)

- updated and applied netplan config for ws2

![upd_conf](../misc/my_images/part2_4.png)

#### 2.1 Adding a static route manually

- add route from ws1 to ws2:

![route_to_ws2](../misc/my_images/part2_5.png)

- add route from ws2 to ws1:

![route_to_ws1](../misc/my_images/part2_6.png)

#### 2.2 Adding a statis route with saving 

- updated netplan config for ws1:

![netplan_ws1](../misc/my_images/part2_7.png)

- updated netplan config for ws2:

![netplan_ws2](../misc/my_images/part2_8.png)

## Part 3 iperf3 utility

#### 3.1 Connectionn speed
#####

    connection speed:
    8 Mpbs = 1 MB/s
    100 MB/s = 800 000 Kbps 
    1 Gbps = 1 000 Mbps

#####

#### 3.2 iperf3 utility 
- measurement of speed connection between ws1 ans ws2

![iperf3 stats](../misc/my_images/part3_1.png)
![iperf3 stats](../misc/my_images/part3_2.png)

## Part 4 Network firewall

#### 4.1 iptables utility 

- example of path of packeats for iptables:

![iptables](../misc/my_images/iptables_flow.png)

- firewall conf for ws1

![firewall](../misc/my_images/part4_1.png)

- firewall conf for ws2

![firewall](../misc/my_images/part4_2.png)

#####

    firewall of ws1 will block any attempt of ping ws1, rules work from top to bottom
    and DROP will work firstly, on ws2 ping will be work beacuse ACCEPT rule goes first

#####

#### 4.2 nmap utility

- output of call `ping` and `nmap`

![check](../misc/my_images/part4_3.png)
![check](../misc/my_images/part4_4.png)

## Part 5 static network routing

#### 5.1 Configuration of machine addresses

- ws11 conf 

![ws11 conf](../misc/my_images/part5_1.png)

- r1 conf

![r1 conf](../misc/my_images/part5_2.png)

- ws21 conf

![ws21 conf](../misc/my_images/part5_3.png)

- ws22 conf

![ws22 conf](../misc/my_images/part5_4.png)

- r2 conf 

![r2 conf](../misc/my_images/part5_5.png)

- call of `netplan apply` and `ip -4 a` 

![ws1](../misc/my_images/part5_6.png)

![r1](../misc/my_images/part5_7.png)

![ws21](../misc/my_images/part5_8.png)

![ws22](../misc/my_images/part5_9.png)

![r2](../misc/my_images/part5_10.png)

- ping r1 from ws11:

![ping r1](../misc/my_images/part5_11.png)

- ping ws22 from ws21:

![ping ws22](../misc/my_images/part5_12.png)


#### 5.2 Enabling ip forwarding 

- call of `sysctl -w inet.ipv4.ip_forward=1` for r1:

![call of `sysctl`](../misc/my_images/part5_13.png)

- call of `sysctl -w inet.ipv4.ip_forward=1` for r2:

![call of `sysctl`](../misc/my_images/part5_14.png)

- example of `sysctl.conf`:

![sysctl_conf](../misc/my_images/part5_15.png)

- update of r1 conf:

![r1 conf](../misc/my_images/part5_16.png)

- update of r2 conf:

![r2 conf](../misc/my_images/part5_17.png)

#### 5.3 Default route configuration:

- netplan conf for ws1:

![ws11_conf](../misc/my_images/part5_18.png)

- netplan conf for ws21:

![ws21_conf](../misc/my_images/part5_19.png)

- netplan conf for ws22:

![ws22_conf](../misc/my_images/part5_20.png)

- call of `ip r` for ws11:

![`ip r` for ws11](../misc/my_images/part5_21.png)

- call of `ip r` for ws21:

![`ip r` for ws21](../misc/my_images/part5_22.png)

- call of `ip r` for ws22:

![`ip r` for ws22](../misc/my_images/part5_23.png)

- ping r2 from ws11:

![ping_r2](../misc/my_images/part5_24.png)

#### 5.4 Adding static routes

- add static route to r1

![r1_route](../misc/my_images/part5_25.png)

- add static route to r2

![r2_route](../misc/my_images/part5_26.png)

- call of `ip r` for r1:

![r1 routes](../misc/my_images/part5_27.png)

- call of `ip r` for r2:

![r2 routes](../misc/my_images/part5_28.png)

- tables of ws11 routes:

#####

    A route other than 0.0.0.0/0 was chosen for the address 10.10.0.0/18 because it is a network address and is reachable without a gateway.

#####


![ws11 routes](../misc/my_images/part5_29.png)

- succesfull ping r2 from ws11 after adding default routes (rem 5.3)

![ping r2](../misc/my_images/part5_30.png)

#### 5.5 Making a router list

- run of tcpdump on r1:

![tcpdump](../misc/my_images/part5_31.png)

- run of traceroute on ws11:

![traceroute](../misc/my_images/part5_32.png)

- dump of traffic of path from ws11 to ws21 on r1:

![traffic_of_dump](../misc/my_images/part5_33.png)

- number of transfered packets:

![nums_of_packets](../misc/my_images/part5_34.png)

#####

    1. The utility uses a specific protocol to send a sequence of IP packets (UPD or ICPM).

    2. By default, the number of packets is limited to 3.

    3. Each subsequent packet is sent with a TTL increment of 1. Thus, the first packet has a TTL of 1, the second has a TTL of 2, and so on.

    4. When transmitted from one router to another, the TTL of each packet is decreased by 1 to prevent the packet from hopping between routers indefinitely.

    5. When a packet's TTL reaches zero, the router discards the packet and sends back an error message.

#####

#### 5.6 Using ICMP protocol

- ping non-existent IP:

![ping_fake_ip](../misc/my_images/part5_35.png)

- run tcpdump and dump of traffic:

![tcpdump](../misc/my_images/part5_36.png)

## Part 6 Dynamic IP configuration using DHCP

- updated config file of dhcpd.conf r2:

![dhcpd_conf](../misc/my_images/part6_1.png)

- updated config of resolv.conf r2:

![resolv_file](../misc/my_images/part6_2.png)

- restart of dhcp daemon r2:
 
![dhcp_daemon](../misc/my_images/part6_3.png)

- new dynamic ip for ws21:

![dynamic_ip_for_ws21](../misc/my_images/part6_4.png)

- adding mac address for ws11:

![mac_address](../misc/my_images/part6_5.png)

- updated config file of dhcpd.conf for r1:

![dhcpd_conf](../misc/my_images/part6_6.png)

- updated resolv.conf for r1:

![resolv_conf](../misc/my_images/part6_7.png)

- restart of dhcp daemon for r1:

![dhcp_daemon](../misc/my_images/part6_8.png)

- error for a new IP on ws11:

![problem_with_dynamic_IP_on_ws11](../misc/my_images/part6_9.png)

- ip for ws21 before requesting new ip:

![old_ip_21](../misc/my_images/part6_10.png)

- the process of requsting new ip for ws21:

![new_ip_21](../misc/my_images/part6_11.png)

- new ip for ws21 after request:

![new_ip](../misc/my_images/part6_12.png)

- `dhclient -r enp0s3` used for freeing an IP address
- `dhclient -v enp0s3` used for request new IP address

## Part 7 NAT

- updated `/etc/apache2/ports.conf` for ws22:

![apache2_ws22](../misc/my_images/part7_1.png)

- updated `/etc/apache2/ports.conf` for r1:

![apache2_r1](../misc/my_images/part7_2.png)

- start of apache2 service for ws22:

![apache2_ws22](../misc/my_images/part7_3.png)

- start of apache2 service for r1:

![apache2_r1](../misc/my_images/part7_4.png)

- updated rules for firewall on r2:

![upd_frwall_r2](../misc/my_images/part7_5.png)

- attempt of ping ws22 from r1:

![ping_ws22](../misc/my_images/part7_6.png)

- adding new rule for firewall on r2:

![upd_frw_r2](../misc/my_images/part7_7.png)

- succesful attemp of ping ws22:

![ping_r2](../misc/my_images/part7_8.png)

- adding rules for SNAT and DNAT for firewall on r2:

![upd_conf_onr2](../misc/my_images/part7_9.png)

- check the TCP connection for SNAT between ws22 and apache server on r1:

![check](../misc/my_images/part7_10.png)

- check the TCP connection for DNAT between between r1 and apache server on ws22:

![check](../misc/my_images/part7_11.png)

