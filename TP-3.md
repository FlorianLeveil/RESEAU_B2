---


---

<h1 id="tp3">TP3</h1>
<h2 id="part.-i">Part. I</h2>
<h3 id="config">CONFIG:</h3>
<p>Switch1:</p>
<pre><code>#CONFIG SWITCH 1

#Config interface 0/1 vlan 10

conf t
vlan 10
name utilisateurs
exit
interface e0/1
switchport mode access
switchport acces vlan 10
exit
#Config interface 0/2 vlan 20

vlan 20
name admins
exit
interface e0/2
switchport mode access
switchport acces vlan 20
exit
#Config interface 0/0 trunk mode

interface e0/0
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40
exit
#Config interface 1/1 trunk mode


interface e1/1
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 20,30,40
exit
exit
</code></pre>
<p>Switch2:</p>
<pre><code>#CONFIG SWITCH 2

#Config interface 0/2 vlan 30

conf t
vlan 30
name visiteurs
exit
interface e0/2
switchport mode access
switchport acces vlan 30
exit

#Config interface 0/1 vlan 40

vlan 40
name imprimantes
exit
interface e0/1
switchport mode access
switchport acces vlan 40
exit

#Config interface 0/3 vlan 20

vlan 20
name admins
exit
interface e0/3
switchport mode access
switchport acces vlan 20
exit

#Config interface 1/1 trunk mode

interface e1/1
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 20,30,40
exit
exit
</code></pre>
<p>Routeur:</p>
<pre><code>conf t
interface ethernet 0/0.10
encapsulation dot1q 10
ip address 10.3.10.254 255.255.255.0
exit


interface ethernet 0/0.20
encapsulation dot1q 20
ip address 10.3.20.254 255.255.255.0
exit

interface ethernet 0/0.30
encapsulation dot1q 30
ip address 10.3.30.254 255.255.255.0
exit

interface ethernet 0/0.40
encapsulation dot1Q 40
ip address 10.3.40.254 255.255.255.0
exit

interface ethernet 0/0
no shut
exit
exit
wr
</code></pre>
<h3 id="tableau-adressage">TABLEAU ADRESSAGE:</h3>

<table>
<thead>
<tr>
<th>Machine</th>
<th>VLAN</th>
<th>IP <code>net1</code></th>
<th>IP <code>net2</code></th>
<th>IP <code>net3</code></th>
<th>IP <code>netP</code></th>
</tr>
</thead>
<tbody>
<tr>
<td>PC1</td>
<td>10</td>
<td><code>10.3.10.1/24</code></td>
<td>x</td>
<td>x</td>
<td>x</td>
</tr>
<tr>
<td>PC2</td>
<td>20</td>
<td>x</td>
<td><code>10.3.20.2/24</code></td>
<td>x</td>
<td>x</td>
</tr>
<tr>
<td>PC3</td>
<td>20</td>
<td>x</td>
<td><code>10.3.20.3/24</code></td>
<td>x</td>
<td>x</td>
</tr>
<tr>
<td>PC4</td>
<td>30</td>
<td>x</td>
<td>x</td>
<td><code>10.3.30.4/24</code></td>
<td>x</td>
</tr>
<tr>
<td>P1</td>
<td>40</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td><code>10.3.40.1/24</code></td>
</tr>
<tr>
<td>R1</td>
<td>x</td>
<td><code>10.3.10.254/24</code></td>
<td><code>10.3.20.254/24</code></td>
<td><code>10.3.30.254/24</code></td>
<td><code>10.3.40.254/24</code></td>
</tr>
</tbody>
</table><h3 id="je-prouve-que-ça-marche">Je prouve que ça marche:</h3>
<p>Switch1:</p>
<pre><code>IOU1#show vlan br

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Et0/3, Et1/0, Et1/2, Et1/3
                                                Et2/0, Et2/1, Et2/2, Et2/3
                                                Et3/0, Et3/1, Et3/2, Et3/3
10   utilisateurs                     active    Et0/1
20   admins                           active    Et0/2
1002 fddi-default                     act/unsup 
1003 token-ring-default               act/unsup 
1004 fddinet-default                  act/unsup 
1005 trnet-default                    act/unsup
</code></pre>
<pre><code>IOU1#show int trunk

Port        Mode             Encapsulation  Status        Native vlan
Et0/0       on               802.1q         trunking      1
Et1/1       on               802.1q         trunking      1

Port        Vlans allowed on trunk
Et0/0       10,20,30,40
Et1/1       20,30,40

Port        Vlans allowed and active in management domain
Et0/0       10,20
Et1/1       20

Port        Vlans in spanning tree forwarding state and not pruned
Et0/0       10,20
Et1/1       20
</code></pre>
<p>Switch2:</p>
<pre><code>IOU2#show vlan br

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Et0/0, Et1/0, Et1/2, Et1/3
                                                Et2/0, Et2/1, Et2/2, Et2/3
                                                Et3/0, Et3/1, Et3/2, Et3/3
20   admins                           active    Et0/3
30   visiteurs                        active    Et0/2
40   imprimantes                      active    Et0/1
1002 fddi-default                     act/unsup 
1003 token-ring-default               act/unsup 
1004 fddinet-default                  act/unsup 
1005 trnet-default                    act/unsup 
</code></pre>
<pre><code>IOU2#show int trunk   

Port        Mode             Encapsulation  Status        Native vlan
Et1/1       on               802.1q         trunking      1

Port        Vlans allowed on trunk
Et1/1       20,30,40

Port        Vlans allowed and active in management domain
Et1/1       20,30,40

Port        Vlans in spanning tree forwarding state and not pruned
Et1/1       20,30,40
</code></pre>
<p>Routeur:</p>
<pre><code>R1#show int trunk

R1#show ip int brief
Interface                  IP-Address      OK? Method Status                Protocol
Ethernet0/0                unassigned      YES NVRAM  up                    up      
Ethernet0/0.10             10.3.10.254     YES NVRAM  up                    up      
Ethernet0/0.20             10.3.20.254     YES NVRAM  up                    up      
Ethernet0/0.30             10.3.30.254     YES NVRAM  up                    up      
Ethernet0/0.40             10.3.40.254     YES NVRAM  up                    up      
Ethernet0/1                unassigned      YES NVRAM  administratively down down    
Ethernet0/2                unassigned      YES NVRAM  administratively down down    
Ethernet0/3                unassigned      YES NVRAM  administratively down down    
R1#
</code></pre>
<p>PC1 PING:</p>
<pre><code>PC-1&gt; ping 10.3.20.2
host (255.255.255.0) not reachable

PC-1&gt; ping 10.3.20.3
host (255.255.255.0) not reachable

PC-1&gt; ping 10.3.30.4
host (255.255.255.0) not reachable

PC-1&gt; ping 10.3.40.1
host (255.255.255.0) not reachable

PC-1&gt; ping 10.3.10.254
84 bytes from 10.3.10.254 icmp_seq=1 ttl=255 time=9.176 ms
84 bytes from 10.3.10.254 icmp_seq=2 ttl=255 time=3.993 ms
</code></pre>
<p>PC2 PING:</p>
<pre><code>PC-2&gt; ping 10.3.10.1
host (255.255.255.0) not reachable

PC-2&gt; ping 10.3.20.3
84 bytes from 10.3.20.3 icmp_seq=1 ttl=64 time=0.495 ms
84 bytes from 10.3.20.3 icmp_seq=2 ttl=64 time=0.692 ms

PC-2&gt; ping 10.3.30.4
84 bytes from 10.3.30.4 icmp_seq=2 ttl=64 time=0.722 ms

PC-2&gt; ping 10.3.20.254
84 bytes from 10.3.20.254 icmp_seq=1 ttl=255 time=9.120 ms

PC-2&gt; ping 10.3.40.1
84 bytes from 10.3.40.1 icmp_seq=2 ttl=255 time=0.822 ms
</code></pre>
<p>PC3 PING:</p>
<pre><code>PC-3&gt; ping 10.3.10.1
host (255.255.255.0) not reachable

PC-3&gt; ping 10.3.20.2
84 bytes from 10.3.20.2 icmp_seq=1 ttl=64 time=0.662 ms
84 bytes from 10.3.20.2 icmp_seq=2 ttl=64 time=0.677 ms

PC-3&gt; ping 10.3.30.4
84 bytes from 10.3.30.4 icmp_seq=2 ttl=255 time=0.982 ms

PC-3&gt; ping 10.3.20.254
84 bytes from 10.3.20.254 icmp_seq=1 ttl=255 time=9.055 ms
84 bytes from 10.3.20.254 icmp_seq=2 ttl=255 time=5.612 ms

PC-3&gt; ping 10.3.40.1
84 bytes from 10.3.40.1 icmp_seq=2 ttl=255 time=0.692 ms
</code></pre>
<p>PC4 PING:</p>
<pre><code>PC-4&gt; ping 10.3.10.1
host (255.255.255.0) not reachable

PC-4&gt; ping 10.3.20.2
84 bytes from 10.3.20.2 icmp_seq=2 ttl=255 time=0.243 ms

PC-4&gt; ping 10.3.20.3
84 bytes from 10.3.20.3 icmp_seq=2 ttl=255 time=0.577 ms

PC-4&gt; ping 10.3.40.1
84 bytes from 10.3.40.1 icmp_seq=2 ttl=255 time=0.742 ms

PC-4&gt; ping 10.3.30.254
84 bytes from 10.3.30.254 icmp_seq=1 ttl=255 time=9.976 ms
84 bytes from 10.3.30.254 icmp_seq=2 ttl=255 time=4.247 ms
</code></pre>
<h2 id="part-.ii">Part .II</h2>
<h3 id="tableau-d’adressage">Tableau d’adressage:</h3>

<table>
<thead>
<tr>
<th>Machine</th>
<th>Net <code>10</code></th>
<th>Net <code>20</code></th>
<th>Net <code>30</code></th>
<th>Net <code>41</code></th>
<th>Net <code>42</code></th>
<th>Net <code>43</code></th>
<th>Net <code>51</code></th>
<th>Net <code>52</code></th>
<th>Net <code>53</code></th>
<th>Net <code>53</code></th>
</tr>
</thead>
<tbody>
<tr>
<td>U1</td>
<td><code>10.1.10.1/24</code></td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
</tr>
<tr>
<td>U2</td>
<td><code>10.1.10.2/24</code></td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
</tr>
<tr>
<td>U3</td>
<td><code>10.1.10.3/24</code></td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
</tr>
<tr>
<td>U4</td>
<td><code>10.1.10.4/24</code></td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
</tr>
<tr>
<td>U5</td>
<td><code>10.1.10.5/24</code></td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
</tr>
<tr>
<td>U6</td>
<td><code>10.1.10.6/24</code></td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
</tr>
<tr>
<td>U7</td>
<td><code>10.1.10.7/24</code></td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
</tr>
<tr>
<td>U8</td>
<td><code>10.1.10.8/24</code></td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
</tr>
<tr>
<td>U9</td>
<td><code>10.1.10.9/24</code></td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
</tr>
<tr>
<td>U10</td>
<td><code>10.1.10.10/24</code></td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
</tr>
<tr>
<td>U11</td>
<td><code>10.1.10.11/24</code></td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
</tr>
<tr>
<td>U12</td>
<td><code>10.1.10.12/24</code></td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
</tr>
<tr>
<td>U13</td>
<td><code>10.1.10.13/24</code></td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
</tr>
<tr>
<td>U14</td>
<td><code>10.1.10.14/24</code></td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
</tr>
<tr>
<td>U15</td>
<td><code>10.1.10.15/24</code></td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
</tr>
<tr>
<td>U16</td>
<td><code>10.1.10.16/24</code></td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
</tr>
<tr>
<td>A1</td>
<td>x</td>
<td><code>10.1.20.1/24</code></td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
</tr>
<tr>
<td>A2</td>
<td>x</td>
<td><code>10.1.20.2/24</code></td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
</tr>
<tr>
<td>A3</td>
<td>x</td>
<td><code>10.1.20.3/24</code></td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
</tr>
<tr>
<td>S1</td>
<td>x</td>
<td>x</td>
<td><code>10.1.30.1/24</code></td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
</tr>
<tr>
<td>S2</td>
<td>x</td>
<td>x</td>
<td><code>10.1.30.2/24</code></td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
</tr>
<tr>
<td>S3</td>
<td>x</td>
<td>x</td>
<td><code>10.1.30.3/24</code></td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
</tr>
<tr>
<td>S4</td>
<td>x</td>
<td>x</td>
<td><code>10.1.30.4/24</code></td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
</tr>
<tr>
<td>S5</td>
<td>x</td>
<td>x</td>
<td><code>10.1.30.5/24</code></td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
</tr>
<tr>
<td>S6</td>
<td>x</td>
<td>x</td>
<td><code>10.1.30.6/24</code></td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
</tr>
<tr>
<td>S7</td>
<td>x</td>
<td>x</td>
<td><code>10.1.30.7/24</code></td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
</tr>
<tr>
<td>S8</td>
<td>x</td>
<td>x</td>
<td><code>10.1.30.8/24</code></td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
</tr>
<tr>
<td>SRV1</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td><code>10.1.42.1/24</code></td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
</tr>
<tr>
<td>SRV2</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td><code>10.1.41.2/24</code></td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
</tr>
<tr>
<td>SRV3</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td><code>10.1.41.3/24</code></td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
</tr>
<tr>
<td>SRV4</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td><code>10.1.43.4/24</code></td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
</tr>
<tr>
<td>SRV5</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td><code>10.1.41.5/24</code></td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
</tr>
<tr>
<td>SRV6</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td><code>10.1.42.6/24</code></td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
</tr>
<tr>
<td>P1</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td><code>10.1.51.1/24</code></td>
<td>x</td>
<td>x</td>
<td>x</td>
</tr>
<tr>
<td>P2</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td><code>10.1.52.2/24</code></td>
<td>x</td>
<td>x</td>
</tr>
<tr>
<td>P3</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td><code>10.1.53.3/24</code></td>
<td>x</td>
</tr>
<tr>
<td>P4</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td><code>10.1.53.4/24</code></td>
<td>x</td>
</tr>
<tr>
<td>P5</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td><code>10.1.54.5/24</code></td>
</tr>
<tr>
<td>R1</td>
<td><code>10.1.10.254/24</code></td>
<td><code>10.1.20.254/24</code></td>
<td><code>10.1.30.254/24</code></td>
<td><code>10.1.41.254/24</code></td>
<td><code>10.1.42.254/24</code></td>
<td><code>10.1.43.254/24</code></td>
<td><code>10.1.51.254/24</code></td>
<td><code>10.1.52.254/24</code></td>
<td><code>10.1.53.254/24</code></td>
<td><code>10.1.54.254/24</code></td>
</tr>
</tbody>
</table><h3 id="conf">Conf:</h3>
<p><strong>U1 à U16:</strong><br>
Même conf juste le nom et l’ip qui change en fonction du PC.<br>
Exemple pour U1:</p>
<pre><code># This the configuration for U1
#
# Uncomment the following line to enable DHCP
# dhcp
# or the line below to manually setup an IP address and subnet mask
ip 10.1.10.1 10.1.10.254
#
set pcname U1
</code></pre>
<p><strong>A1 à A3:</strong><br>
Même conf juste le nom et l’ip qui change en fonction du PC.<br>
Exemple pour A1:</p>
<pre><code># This the configuration for A1
#
# Uncomment the following line to enable DHCP
# dhcp
# or the line below to manually setup an IP address and subnet mask
ip 10.1.20.1 10.1.20.254
#
set pcname A1
</code></pre>
<p><strong>S1 à S8:</strong><br>
Même conf juste le nom et l’ip qui change en fonction du PC.<br>
Exemple pour S1:</p>
<pre><code># This the configuration for S1
#
# Uncomment the following line to enable DHCP
# dhcp
# or the line below to manually setup an IP address and subnet mask
ip 10.1.30.1 10.1.30.254
#
set pcname S8
</code></pre>
<p><strong>SRV2, SRV3 et SRV5:</strong><br>
Même conf juste le nom et l’ip qui change en fonction du SRV.<br>
Exemple pour SRV2:</p>
<pre><code># This the configuration for SRV2
#
# Uncomment the following line to enable DHCP
# dhcp
# or the line below to manually setup an IP address and subnet mask
ip 10.1.41.2 10.1.41.254
#
set pcname SRV2
</code></pre>
<p><strong>SRV1 et SRV6:</strong><br>
Même conf juste le nom et l’ip qui change en fonction du SRV.<br>
Exemple pour SRV1:</p>
<pre><code># This the configuration for SRV1
#
# Uncomment the following line to enable DHCP
# dhcp
# or the line below to manually setup an IP address and subnet mask
ip 10.1.42.1 10.1.42.254
#
set pcname SRV1
</code></pre>
<p><strong>SRV4:</strong></p>
<pre><code># This the configuration for SRV4
#
# Uncomment the following line to enable DHCP
# dhcp
# or the line below to manually setup an IP address and subnet mask
ip 10.1.43.4 10.1.43.254
#
set pcname SRV4
</code></pre>
<p><strong>P1:</strong></p>
<pre><code># This the configuration for P1
#
# Uncomment the following line to enable DHCP
# dhcp
# or the line below to manually setup an IP address and subnet mask
ip 10.1.51.1 10.1.51.254
#
set pcname P1
</code></pre>
<p><strong>P2:</strong></p>
<pre><code># This the configuration for P2
#
# Uncomment the following line to enable DHCP
# dhcp
# or the line below to manually setup an IP address and subnet mask
ip 10.1.52.2 10.1.52.254
#
set pcname P2
</code></pre>
<p><strong>P3 et P4:</strong><br>
Même conf juste le nom et l’ip qui change en fonction de l’imprimante.<br>
Exemple pour P3:</p>
<pre><code># This the configuration for P3
#
# Uncomment the following line to enable DHCP
# dhcp
# or the line below to manually setup an IP address and subnet mask
ip 10.1.53.3 10.1.53.254
#
set pcname P3
</code></pre>
<p><strong>P5:</strong></p>
<pre><code># This the configuration for P5
#
# Uncomment the following line to enable DHCP
# dhcp
# or the line below to manually setup an IP address and subnet mask
ip 10.1.54.5 10.1.54.254
#
set pcname P5
</code></pre>
<p><strong>SW1:</strong></p>
<pre><code>#Config vlan 10 utilisateur

conf t
vlan 10
name utilisateurs
exit
interface e0/1
switchport mode access
switchport acces vlan 10
exit
interface e0/2
switchport mode access
switchport acces vlan 10
exit
interface e0/3
switchport mode access
switchport acces vlan 10
exit
interface e0/4
switchport mode access
switchport acces vlan 10
exit
interface e1/1
switchport mode access
switchport acces vlan 10
exit
interface e1/2
switchport mode access
switchport acces vlan 10
exit


#Config vlan 30 stage
vlan 30
name stage
exit
interface e2/1
switchport mode access
switchport acces vlan 30
exit
interface e2/2
switchport mode access
switchport acces vlan 30
exit

#Config vlan 51 imp
vlan 51
name imp
exit
interface e4/1
switchport mode access
switchport acces vlan 51
exit


#Config interface 3/1 trunk mode vers le switch
interface e3/1
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20,30,41,51
exit
exit
</code></pre>
<p><strong>SW2:</strong></p>
<pre><code>#Config vlan 10 utilisateur
conf t
vlan 10
name utilisateurs
exit
interface e0/1
switchport mode access
switchport acces vlan 10
exit
interface e0/2
switchport mode access
switchport acces vlan 10
exit
interface e0/3
switchport mode access
switchport acces vlan 10
exit
interface e0/4
switchport mode access
switchport acces vlan 10
exit

#Config vlan 20 admin
vlan 20
name admin
exit
interface e2/1
switchport mode access
switchport acces vlan 20
exit

#Config vlan 30 stage
vlan 30
name stage
exit
interface e3/1
switchport mode access
switchport acces vlan 30
exit
interface e3/2
switchport mode access
switchport acces vlan 30
exit
interface e3/3
switchport mode access
switchport acces vlan 30
exit

#Config vlan 52 imp
vlan 52
name imp
exit
interface e4/1
switchport mode access
switchport acces vlan 52
exit

#Config interface 4/2 trunk mode vers le switch
interface e4/2
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20,30,41,42,52,51,53,54
exit
exit
</code></pre>
<p><strong>SW3:</strong></p>
<pre><code>#Config vlan 10 utilisateur
conf t
vlan 10
name utilisateurs
exit
interface e0/1
switchport mode access
switchport acces vlan 10
exit
interface e0/2
switchport mode access
switchport acces vlan 10
exit
interface e0/3
switchport mode access
switchport acces vlan 10
exit
interface e0/4
switchport mode access
switchport acces vlan 10
exit
interface e1/1
switchport mode access
switchport acces vlan 10
exit
interface e1/2
switchport mode access
switchport acces vlan 10
exit

#Config vlan 20 admin
vlan 20
name admin
exit
interface e2/1
switchport mode access
switchport acces vlan 20
exit

#Config vlan 30 stage
vlan 30
name stage
exit
interface e3/1
switchport mode access
switchport acces vlan 30
exit
interface e3/2
switchport mode access
switchport acces vlan 30
exit
interface e3/3
switchport mode access
switchport acces vlan 30
exit

#Config vlan 53 imp
vlan 53
name imp
exit
interface e4/1
switchport mode access
switchport acces vlan 53
exit
interface e4/2
switchport mode access
switchport acces vlan 53
exit

#Config interface 4/3 trunk mode vers le switch
interface e4/3
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20,30,41,42,53,51,52,54
exit
exit
</code></pre>
<p><strong>SW4:</strong></p>
<pre><code>#Config vlan 41 serveur classique
conf t
vlan 41
name serveurc
exit
interface e1/1
switchport mode access
switchport acces vlan 41
exit
interface e1/2
switchport mode access
switchport acces vlan 41
exit
interface e1/3
switchport mode access
switchport acces vlan 41
exit

#Config vlan 42 serveur admin
vlan 42
name serveura
exit
interface e2/1
switchport mode access
switchport acces vlan 42
exit
interface e2/2
switchport mode access
switchport acces vlan 42
exit

#Config vlan 43 serveur admin spe
vlan 43
name serveuras
exit
interface e3/1
switchport mode access
switchport acces vlan 43
exit


#Config interface 4/1 trunk mode vers le switch
interface e4/1
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20,30,41,42,43
exit
exit
</code></pre>
<p><strong>SW5:</strong></p>
<pre><code>#Config vlan 20 admin
conf t
vlan 20
name admin
exit
interface e1/1
switchport mode access
switchport acces vlan 20
exit

#Config vlan 54 imp
vlan 54
name imp
exit
interface e2/1
switchport mode access
switchport acces vlan 54
exit

#Config interface 3/1 trunk mode vers le switch
interface e3/1
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 20,41,42,43,51,52,53,54
exit
exit
</code></pre>
<p><strong>R1</strong></p>
<pre><code>
</code></pre>
<h3 id="topo">Topo:</h3>
<img src="https://github.com/FlorianLeveil/RESEAU_B2/blob/master/Images/modiftp3.png" alt="">

