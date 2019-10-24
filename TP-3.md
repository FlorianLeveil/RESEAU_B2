---


---

<h1 id="tp3">TP3</h1>
<h2 id="part-i">Part I</h2>
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

