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

