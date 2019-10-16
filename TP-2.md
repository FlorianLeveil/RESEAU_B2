---


---

<h1 id="i.-simplest-setup">I. Simplest setup</h1>
<h4 id="topologie">TOPOLOGIE</h4>
<p><img src="https://github.com/FlorianLeveil/RESEAU_B2/blob/master/Images/topo1.png" alt=""><br>
Ci dessous nous pouvons constater les échanges ARP:</p>
<pre><code>No.     Time           Source                Destination           Protocol Length Info
    299 503.953067     Private_66:68:00      Broadcast             ARP      64     Who has 10.2.1.2? Tell 10.2.1.1

Frame 299: 64 bytes on wire (512 bits), 64 bytes captured (512 bits)
Ethernet II, Src: Private_66:68:00 (00:50:79:66:68:00), Dst: Broadcast (ff:ff:ff:ff:ff:ff)
Address Resolution Protocol (request)

No.     Time           Source                Destination           Protocol Length Info
    300 503.954869     Private_66:68:01      Private_66:68:00      ARP      64     10.2.1.2 is at 00:50:79:66:68:01

Frame 300: 64 bytes on wire (512 bits), 64 bytes captured (512 bits)
Ethernet II, Src: Private_66:68:01 (00:50:79:66:68:01), Dst: Private_66:68:00 (00:50:79:66:68:00)
Address Resolution Protocol (reply)

No.     Time           Source                Destination           Protocol Length Info
    301 503.954984     Private_66:68:01      Private_66:68:00      ARP      64     10.2.1.2 is at 00:50:79:66:68:01

Frame 301: 64 bytes on wire (512 bits), 64 bytes captured (512 bits)
Ethernet II, Src: Private_66:68:01 (00:50:79:66:68:01), Dst: Private_66:68:00 (00:50:79:66:68:00)
Address Resolution Protocol (reply)
</code></pre>
<p>Une fois l’échange ARP fait,  on peux voir le ping se faire:<br>
Protocole: ICMP (Type 0 = Reply | Type 8 = Request)</p>
<pre><code>No.     Time           Source                Destination           Protocol Length Info
    302 503.955299     10.2.1.1              10.2.1.2              ICMP     98     Echo (ping) request  id=0xf800, seq=1/256, ttl=64 (reply in 303)

Frame 302: 98 bytes on wire (784 bits), 98 bytes captured (784 bits)
Ethernet II, Src: Private_66:68:00 (00:50:79:66:68:00), Dst: Private_66:68:01 (00:50:79:66:68:01)
Internet Protocol Version 4, Src: 10.2.1.1, Dst: 10.2.1.2
Internet Control Message Protocol

No.     Time           Source                Destination           Protocol Length Info
    303 503.955622     10.2.1.2              10.2.1.1              ICMP     98     Echo (ping) reply    id=0xf800, seq=1/256, ttl=64 (request in 302)

Frame 303: 98 bytes on wire (784 bits), 98 bytes captured (784 bits)
Ethernet II, Src: Private_66:68:01 (00:50:79:66:68:01), Dst: Private_66:68:00 (00:50:79:66:68:00)
Internet Protocol Version 4, Src: 10.2.1.2, Dst: 10.2.1.1
Internet Control Message Protocol
</code></pre>
<p>Table ARP:</p>
<pre><code>PC-2&gt; show arp

00:50:79:66:68:00  10.2.1.1 expires in 98 seconds 
</code></pre>
<pre><code>PC-1&gt; show arp      

00:50:79:66:68:01  10.2.1.2 expires in 101 seconds 
</code></pre>
<p>🌞  Récapitulation des étapes d’un ping PC-1 vers PC-2:</p>
<ul>
<li>1: PC-1 envoie un message sous le protocole ARP pour destination Broacast <code>Qui est 10.2.1.2 ?</code></li>
<li>2: La Broacast répond <code>10.2.1.2 est à 00:50:79:66:68:01</code></li>
<li>3: PC-1 envoie un message sous le protocole ICMP type 8 à PC-2</li>
<li>4: PC-2 envoie un message sous le protocole ICMP type 0 à PC-1</li>
</ul>
<p>🌞 Explication:</p>
<ul>
<li>
<p>Pourquoi le switch n’a pas besoin d’IP:<br>
Par ce qu’il sert uniquement de relais !</p>
</li>
<li>
<p>Pourquoi les machines ont besoin d’une IP pour pouvoir se <code>ping</code>:<br>
Par ce que la commande <code>ping</code> utilise le protocole ICMP qui lui utilise le protocole IP.</p>
</li>
</ul>
<h1 id="ii.-more-switches">II. More switches</h1>
<h4 id="topologie-1">Topologie</h4>
<pre><code>                        +-----+
                        | PC2 |
                        +--+--+
                           |
                           |
                       +---+---+
                   +---+  SW2  +----+
                   |   +-------+    |
                   |                |
                   |                |
+-----+        +---+---+        +---+---+        +-----+
| PC1 +--------+  SW1  +--------+  SW3  +--------+ PC3 |
+-----+        +-------+        +-------+        +-----+
</code></pre>
<h4 id="todo">ToDo</h4>
<p><img src="https://github.com/FlorianLeveil/RESEAU_B2/blob/master/Images/topo2.png" alt=""></p>
<h3 id="🌞-faire-communiquer-les-trois-pc">🌞 faire communiquer les trois PC:</h3>
<p>PC1 vers PC2:</p>
<pre><code>No.     Time           Source                Destination           Protocol Length Info
     14 14.872121      Private_66:68:02      Broadcast             ARP      64     Who has 10.2.2.2? Tell 10.2.2.1
     15 14.876550      Private_66:68:03      Private_66:68:02      ARP      64     10.2.2.2 is at 00:50:79:66:68:03
     16 14.876724      Private_66:68:03      Private_66:68:02      ARP      64     10.2.2.2 is at 00:50:79:66:68:03
     17 14.876877      Private_66:68:03      Private_66:68:02      ARP      64     10.2.2.2 is at 00:50:79:66:68:03
     18 14.877028      Private_66:68:03      Private_66:68:02      ARP      64     10.2.2.2 is at 00:50:79:66:68:03
     19 14.878046      10.2.2.1              10.2.2.2              ICMP     98     Echo (ping) request  id=0x361c, seq=1/256, ttl=64 (reply in 20)
     20 14.878515      10.2.2.2              10.2.2.1              ICMP     98     Echo (ping) reply    id=0x361c, seq=1/256, ttl=64 (request in 19)
     21 15.879190      10.2.2.1              10.2.2.2              ICMP     98     Echo (ping) request  id=0x371c, seq=2/512, ttl=64 (reply in 22)
     22 15.879469      10.2.2.2              10.2.2.1              ICMP     98     Echo (ping) reply    id=0x371c, seq=2/512, ttl=64 (request in 21)

</code></pre>
<p>PC1 vers PC3:</p>
<pre><code>No.     Time           Source                Destination           Protocol Length Info
     28 20.548025      Private_66:68:02      Broadcast             ARP      64     Who has 10.2.2.3? Tell 10.2.2.1
     29 20.551440      Private_66:68:04      Private_66:68:02      ARP      64     10.2.2.3 is at 00:50:79:66:68:04
     30 20.552067      Private_66:68:04      Private_66:68:02      ARP      64     10.2.2.3 is at 00:50:79:66:68:04
     31 20.552201      Private_66:68:04      Private_66:68:02      ARP      64     10.2.2.3 is at 00:50:79:66:68:04
     32 20.553272      Private_66:68:04      Private_66:68:02      ARP      64     10.2.2.3 is at 00:50:79:66:68:04
     33 20.553381      Private_66:68:04      Private_66:68:02      ARP      64     10.2.2.3 is at 00:50:79:66:68:04
     34 20.553486      Private_66:68:04      Private_66:68:02      ARP      64     10.2.2.3 is at 00:50:79:66:68:04
     35 20.553606      Private_66:68:04      Private_66:68:02      ARP      64     10.2.2.3 is at 00:50:79:66:68:04
     36 20.553983      Private_66:68:04      Private_66:68:02      ARP      64     10.2.2.3 is at 00:50:79:66:68:04
     37 20.554422      10.2.2.1              10.2.2.3              ICMP     98     Echo (ping) request  id=0x3c1c, seq=1/256, ttl=64 (reply in 38)
     38 20.554660      10.2.2.3              10.2.2.1              ICMP     98     Echo (ping) reply    id=0x3c1c, seq=1/256, ttl=64 (request in 37)
     39 21.555589      10.2.2.1              10.2.2.3              ICMP     98     Echo (ping) request  id=0x3d1c, seq=2/512, ttl=64 (reply in 40)
     40 21.556344      10.2.2.3              10.2.2.1              ICMP     98     Echo (ping) reply    id=0x3d1c, seq=2/512, ttl=64 (request in 39)
     41 22.091327      aa:bb:cc:00:04:00     Spanning-tree-(for-bridges)_00 STP      60     RST. Root = 32768/1/aa:bb:cc:00:02:00  Cost = 100  Port = 0x8001
     42 22.556911      10.2.2.1              10.2.2.3              ICMP     98     Echo (ping) request  id=0x3e1c, seq=3/768, ttl=64 (reply in 43)
     43 22.557590      10.2.2.3              10.2.2.1              ICMP     98     Echo (ping) reply    id=0x3e1c, seq=3/768, ttl=64 (request in 42)
</code></pre>
<p>PC2 vers PC1:</p>
<pre><code>No.     Time           Source                Destination           Protocol Length Info
     15 17.644305      Private_66:68:03      Broadcast             ARP      64     Who has 10.2.2.1? Tell 10.2.2.2
     16 17.648409      Private_66:68:02      Private_66:68:03      ARP      64     10.2.2.1 is at 00:50:79:66:68:02
     17 17.648600      Private_66:68:02      Private_66:68:03      ARP      64     10.2.2.1 is at 00:50:79:66:68:02
     18 17.648766      Private_66:68:02      Private_66:68:03      ARP      64     10.2.2.1 is at 00:50:79:66:68:02
     19 17.648929      Private_66:68:02      Private_66:68:03      ARP      64     10.2.2.1 is at 00:50:79:66:68:02
     20 17.650207      10.2.2.2              10.2.2.1              ICMP     98     Echo (ping) request  id=0x7525, seq=1/256, ttl=64 (reply in 21)
     21 17.650958      10.2.2.1              10.2.2.2              ICMP     98     Echo (ping) reply    id=0x7525, seq=1/256, ttl=64 (request in 20)
     22 18.063916      aa:bb:cc:00:02:00     Spanning-tree-(for-bridges)_00 STP      60     RST. Root = 32768/1/aa:bb:cc:00:02:00  Cost = 0  Port = 0x8001
     23 18.652338      10.2.2.2              10.2.2.1              ICMP     98     Echo (ping) request  id=0x7625, seq=2/512, ttl=64 (reply in 24)
     24 18.652980      10.2.2.1              10.2.2.2              ICMP     98     Echo (ping) reply    id=0x7625, seq=2/512, ttl=64 (request in 23)

</code></pre>
<p>PC2 vers PC3:</p>
<pre><code>No.     Time           Source                Destination           Protocol Length Info
     27 22.512248      Private_66:68:03      Broadcast             ARP      64     Who has 10.2.2.3? Tell 10.2.2.2
     28 22.515344      Private_66:68:04      Private_66:68:03      ARP      64     10.2.2.3 is at 00:50:79:66:68:04
     29 22.515506      Private_66:68:04      Private_66:68:03      ARP      64     10.2.2.3 is at 00:50:79:66:68:04
     30 22.515649      Private_66:68:04      Private_66:68:03      ARP      64     10.2.2.3 is at 00:50:79:66:68:04
     31 22.515783      Private_66:68:04      Private_66:68:03      ARP      64     10.2.2.3 is at 00:50:79:66:68:04
     32 22.516675      10.2.2.2              10.2.2.3              ICMP     98     Echo (ping) request  id=0x7a25, seq=1/256, ttl=64 (reply in 33)
     33 22.517076      10.2.2.3              10.2.2.2              ICMP     98     Echo (ping) reply    id=0x7a25, seq=1/256, ttl=64 (request in 32)
     34 23.517856      10.2.2.2              10.2.2.3              ICMP     98     Echo (ping) request  id=0x7b25, seq=2/512, ttl=64 (reply in 35)
     35 23.518441      10.2.2.3              10.2.2.2              ICMP     98     Echo (ping) reply    id=0x7b25, seq=2/512, ttl=64 (request in 34)
     36 24.092240      aa:bb:cc:00:02:00     Spanning-tree-(for-bridges)_00 STP      60     RST. Root = 32768/1/aa:bb:cc:00:02:00  Cost = 0  Port = 0x8001
     37 24.519912      10.2.2.2              10.2.2.3              ICMP     98     Echo (ping) request  id=0x7c25, seq=3/768, ttl=64 (reply in 38)

</code></pre>
<p>PC3 vers PC1:</p>
<pre><code>No.     Time           Source                Destination           Protocol Length Info
      9 5.606954       Private_66:68:04      Broadcast             ARP      64     Who has 10.2.2.1? Tell 10.2.2.3
     10 5.611006       Private_66:68:02      Private_66:68:04      ARP      64     10.2.2.1 is at 00:50:79:66:68:02
     11 5.611230       Private_66:68:02      Private_66:68:04      ARP      64     10.2.2.1 is at 00:50:79:66:68:02
     12 5.611431       Private_66:68:02      Private_66:68:04      ARP      64     10.2.2.1 is at 00:50:79:66:68:02
     13 5.611662       Private_66:68:02      Private_66:68:04      ARP      64     10.2.2.1 is at 00:50:79:66:68:02
     14 5.611877       Private_66:68:02      Private_66:68:04      ARP      64     10.2.2.1 is at 00:50:79:66:68:02
     15 5.612108       Private_66:68:02      Private_66:68:04      ARP      64     10.2.2.1 is at 00:50:79:66:68:02
     16 5.612314       Private_66:68:02      Private_66:68:04      ARP      64     10.2.2.1 is at 00:50:79:66:68:02
     17 5.612519       Private_66:68:02      Private_66:68:04      ARP      64     10.2.2.1 is at 00:50:79:66:68:02
     18 5.613163       10.2.2.3              10.2.2.1              ICMP     98     Echo (ping) request  id=0xa226, seq=1/256, ttl=64 (reply in 19)
     19 5.613645       10.2.2.1              10.2.2.3              ICMP     98     Echo (ping) reply    id=0xa226, seq=1/256, ttl=64 (request in 18)
     20 6.012662       aa:bb:cc:00:03:00     Spanning-tree-(for-bridges)_00 STP      60     RST. Root = 32768/1/aa:bb:cc:00:02:00  Cost = 100  Port = 0x8001
     21 6.614360       10.2.2.3              10.2.2.1              ICMP     98     Echo (ping) request  id=0xa326, seq=2/512, ttl=64 (reply in 22)
     22 6.614763       10.2.2.1              10.2.2.3              ICMP     98     Echo (ping) reply    id=0xa326, seq=2/512, ttl=64 (request in 21)
     23 7.615576       10.2.2.3              10.2.2.1              ICMP     98     Echo (ping) request  id=0xa426, seq=3/768, ttl=64 (reply in 24)
     24 7.616386       10.2.2.1              10.2.2.3              ICMP     98     Echo (ping) reply    id=0xa426, seq=3/768, ttl=64 (request in 23)
</code></pre>
<p>PC3 vers PC2:</p>
<pre><code>No.     Time           Source                Destination           Protocol Length Info
     27 10.278943      Private_66:68:04      Broadcast             ARP      64     Who has 10.2.2.2? Tell 10.2.2.3
     28 10.283187      Private_66:68:03      Private_66:68:04      ARP      64     10.2.2.2 is at 00:50:79:66:68:03
     29 10.283402      Private_66:68:03      Private_66:68:04      ARP      64     10.2.2.2 is at 00:50:79:66:68:03
     30 10.283686      10.2.2.3              10.2.2.2              ICMP     98     Echo (ping) request  id=0xa726, seq=1/256, ttl=64 (reply in 33)
     31 10.283827      Private_66:68:03      Private_66:68:04      ARP      64     10.2.2.2 is at 00:50:79:66:68:03
     32 10.283940      Private_66:68:03      Private_66:68:04      ARP      64     10.2.2.2 is at 00:50:79:66:68:03
     33 10.284175      10.2.2.2              10.2.2.3              ICMP     98     Echo (ping) reply    id=0xa726, seq=1/256, ttl=64 (request in 30)
     34 11.284917      10.2.2.3              10.2.2.2              ICMP     98     Echo (ping) request  id=0xa826, seq=2/512, ttl=64 (reply in 35)
     35 11.285672      10.2.2.2              10.2.2.3              ICMP     98     Echo (ping) reply    id=0xa826, seq=2/512, ttl=64 (request in 34)
     36 12.028748      aa:bb:cc:00:03:00     Spanning-tree-(for-bridges)_00 STP      60     RST. Root = 32768/1/aa:bb:cc:00:02:00  Cost = 100  Port = 0x8001
     37 12.286134      10.2.2.3              10.2.2.2              ICMP     98     Echo (ping) request  id=0xa926, seq=3/768, ttl=64 (reply in 38)
     38 12.286925      10.2.2.2              10.2.2.3              ICMP     98     Echo (ping) reply    id=0xa926, seq=3/768, ttl=64 (request in 37)
</code></pre>
<ul>
<li>avec des <code>ping</code> qui fonctionnent</li>
</ul>
<h3 id="🌞-analyser-la-table-mac-dun-switch">🌞 Analyser la table MAC d’un switch</h3>
<ul>
<li>SWITCH 1:</li>
</ul>
<pre><code>IOU-1#show mac address-table
          Mac Address Table
-------------------------------------------

Vlan    Mac Address       Type        Ports
----    -----------       --------    -----
   1    0050.7966.6802    DYNAMIC     Et0/0
   1    0050.7966.6803    DYNAMIC     Et0/1
   1    0050.7966.6804    DYNAMIC     Et0/1
   1    aabb.cc00.0210    DYNAMIC     Et0/1
   1    aabb.cc00.0330    DYNAMIC     Et0/1
   1    aabb.cc00.0420    DYNAMIC     Et0/1
Total Mac Addresses for this criterion: 6
</code></pre>
<ul>
<li>SWITCH 2:</li>
</ul>
<pre><code>IOU-2#show mac address-table
          Mac Address Table
-------------------------------------------

Vlan    Mac Address       Type        Ports
----    -----------       --------    -----
   1    0050.7966.6802    DYNAMIC     Et0/1
   1    0050.7966.6803    DYNAMIC     Et0/0
   1    0050.7966.6804    DYNAMIC     Et0/3
   1    aabb.cc00.0330    DYNAMIC     Et0/3
   1    aabb.cc00.0410    DYNAMIC     Et0/1
   1    aabb.cc00.0420    DYNAMIC     Et0/3
Total Mac Addresses for this criterion: 6
</code></pre>
<ul>
<li>SWITCH 3:</li>
</ul>
<pre><code>IOU-3#show mac address-table
          Mac Address Table
-------------------------------------------

Vlan    Mac Address       Type        Ports
----    -----------       --------    -----
   1    0050.7966.6802    DYNAMIC     Et0/3
   1    0050.7966.6803    DYNAMIC     Et0/3
   1    0050.7966.6804    DYNAMIC     Et0/0
   1    aabb.cc00.0230    DYNAMIC     Et0/3
   1    aabb.cc00.0410    DYNAMIC     Et0/3
   1    aabb.cc00.0420    DYNAMIC     Et0/2
Total Mac Addresses for this criterion: 6
</code></pre>
<ul>
<li>comprendre/expliquer chaque ligne:</li>
</ul>
<p>Vlan: Le numéro du Vlan dans lequel est situé l’appareil.<br>
Mac Address: L’adresse mac de l’appareil<br>
Type: En fonction de comment les machines sont configuré. Elles peuvent être dynamic ou static.<br>
Ports: Ce sont les ports sur lesquels les machines sont branchées.</p>
<h3 id="le-stp-sil-te-plaît--on-rigole-bien.">Le STP (S’il te plaît ) <em>on rigole bien.</em></h3>
<ul>
<li>🌞 déterminer les informations STP</li>
</ul>
<ul>
<li>SWITCH 1</li>
</ul>
<pre><code>IOU-1#show spanning-tree 

VLAN0001
  Spanning tree enabled protocol rstp
  Root ID    Priority    32769
             Address     aabb.cc00.0200
             Cost        100
             Port        2 (Ethernet0/1)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     aabb.cc00.0400
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Et0/0               Desg FWD 100       128.1    Shr 
Et0/1               Root FWD 100       128.2    Shr 
Et0/2               Altn BLK 100       128.3    Shr 
Et0/3               Desg FWD 100       128.4    Shr 
Et1/0               Desg FWD 100       128.5    Shr 
Et1/1               Desg FWD 100       128.6    Shr 
Et1/2               Desg FWD 100       128.7    Shr 
Et1/3               Desg FWD 100       128.8    Shr 
Et2/0               Desg FWD 100       128.9    Shr 
Et2/1               Desg FWD 100       128.10   Shr 
Et2/2               Desg FWD 100       128.11   Shr 
Et2/3               Desg FWD 100       128.12   Shr 
Et3/0               Desg FWD 100       128.13   Shr 
Et3/1               Desg FWD 100       128.14   Shr 
Et3/2               Desg FWD 100       128.15   Shr 
Et3/3               Desg FWD 100       128.16   Shr 
</code></pre>
<ul>
<li>SWITCH 2</li>
</ul>
<pre><code>IOU-2#show spanning-tree 

VLAN0001
  Spanning tree enabled protocol rstp
  Root ID    Priority    32769
             Address     aabb.cc00.0200
             This bridge is the root
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     aabb.cc00.0200
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Et0/0               Desg FWD 100       128.1    Shr 
Et0/1               Desg FWD 100       128.2    Shr 
Et0/2               Desg FWD 100       128.3    Shr 
Et0/3               Desg FWD 100       128.4    Shr 
Et1/0               Desg FWD 100       128.5    Shr 
Et1/1               Desg FWD 100       128.6    Shr 
Et1/2               Desg FWD 100       128.7    Shr 
Et1/3               Desg FWD 100       128.8    Shr 
Et2/0               Desg FWD 100       128.9    Shr 
Et2/1               Desg FWD 100       128.10   Shr 
Et2/2               Desg FWD 100       128.11   Shr 
Et2/3               Desg FWD 100       128.12   Shr 
Et3/0               Desg FWD 100       128.13   Shr 
Et3/1               Desg FWD 100       128.14   Shr 
Et3/2               Desg FWD 100       128.15   Shr 
Et3/3               Desg FWD 100       128.16   Shr 
</code></pre>
<ul>
<li>SWITCH 3</li>
</ul>
<pre><code>IOU-3#show spanning-tree 

VLAN0001
  Spanning tree enabled protocol rstp
  Root ID    Priority    32769
             Address     aabb.cc00.0200
             Cost        100
             Port        4 (Ethernet0/3)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     aabb.cc00.0300
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Et0/0               Desg FWD 100       128.1    Shr 
Et0/1               Desg FWD 100       128.2    Shr 
Et0/2               Desg FWD 100       128.3    Shr 
Et0/3               Root FWD 100       128.4    Shr 
Et1/0               Desg FWD 100       128.5    Shr 
Et1/1               Desg FWD 100       128.6    Shr 
Et1/2               Desg FWD 100       128.7    Shr 
Et1/3               Desg FWD 100       128.8    Shr 
Et2/0               Desg FWD 100       128.9    Shr 
Et2/1               Desg FWD 100       128.10   Shr 
Et2/2               Desg FWD 100       128.11   Shr 
Et2/3               Desg FWD 100       128.12   Shr 
Et3/0               Desg FWD 100       128.13   Shr 
Et3/1               Desg FWD 100       128.14   Shr 
Et3/2               Desg FWD 100       128.15   Shr 
Et3/3               Desg FWD 100       128.16   Shr 
</code></pre>
<p>Comme on peut le voir ci-dessus il y marqué sur le SWITCH 2: <code>This bridge is the root</code>.</p>
<p>🌞 Schéma représentant les informations STP</p>

<table>
<thead>
<tr>
<th></th>
<th align="center">SW2-1</th>
<th align="center">SW2-2</th>
<th align="center">SW2-3</th>
</tr>
</thead>
<tbody>
<tr>
<td>Rôle</td>
<td align="center"><code>Root Bridge</code></td>
<td align="center">Switch</td>
<td align="center">Switch</td>
</tr>
<tr>
<td>Root Port</td>
<td align="center">X</td>
<td align="center"><code>e0/1</code></td>
<td align="center"><code>e0/1</code></td>
</tr>
<tr>
<td>Port désactivé</td>
<td align="center">X</td>
<td align="center">X</td>
<td align="center"><code>e0/2</code></td>
</tr>
</tbody>
</table><p><img src="https://github.com/FlorianLeveil/RESEAU_B2/blob/master/Images/topo2.png" alt=""><br>
🌞 confirmer les informations STP</p>
<pre><code>[idk@localhost ~]$ cat ./COURS/RESEAU/TP2/idk1.txt 
No.     Time           Source                Destination           Protocol Length Info
      1 0.000000       aa:bb:cc:00:02:30     CDP/VTP/DTP/PAgP/UDLD DTP      90     Dynamic Trunk Protocol
      2 0.058880       aa:bb:cc:00:04:20     CDP/VTP/DTP/PAgP/UDLD DTP      90     Dynamic Trunk Protocol
      3 0.060001       aa:bb:cc:00:04:10     CDP/VTP/DTP/PAgP/UDLD DTP      90     Dynamic Trunk Protocol
      4 0.089766       aa:bb:cc:00:03:00     CDP/VTP/DTP/PAgP/UDLD DTP      60     Dynamic Trunk Protocol
      5 0.089811       aa:bb:cc:00:03:00     CDP/VTP/DTP/PAgP/UDLD DTP      90     Dynamic Trunk Protocol
      6 1.364844       aa:bb:cc:00:03:00     Spanning-tree-(for-bridges)_00 STP      60     RST. TC + Root = 32768/1/aa:bb:cc:00:02:00  Cost = 100  Port = 0x8001
      7 3.375980       aa:bb:cc:00:03:00     Spanning-tree-(for-bridges)_00 STP      60     RST. TC + Root = 32768/1/aa:bb:cc:00:02:00  Cost = 100  Port = 0x8001
      8 5.380596       aa:bb:cc:00:03:00     Spanning-tree-(for-bridges)_00 STP      60     RST. Root = 32768/1/aa:bb:cc:00:02:00  Cost = 100  Port = 0x8001
      9 7.389070       aa:bb:cc:00:03:00     Spanning-tree-(for-bridges)_00 STP      60     RST. Root = 32768/1/aa:bb:cc:00:02:00  Cost = 100  Port = 0x8001
     10 9.397085       aa:bb:cc:00:03:00     Spanning-tree-(for-bridges)_00 STP      60     RST. Root = 32768/1/aa:bb:cc:00:02:00  Cost = 100  Port = 0x8001
     11 10.786508      Private_66:68:04      Broadcast             ARP      64     Who has 10.2.2.2? Tell 10.2.2.3
     12 10.791429      Private_66:68:03      Private_66:68:04      ARP      64     10.2.2.2 is at 00:50:79:66:68:03
     13 10.791721      Private_66:68:03      Private_66:68:04      ARP      64     10.2.2.2 is at 00:50:79:66:68:03
     14 10.793627      Private_66:68:03      Private_66:68:04      ARP      64     10.2.2.2 is at 00:50:79:66:68:03
     15 10.793816      Private_66:68:03      Private_66:68:04      ARP      64     10.2.2.2 is at 00:50:79:66:68:03
     16 10.794915      10.2.2.3              10.2.2.2              ICMP     98     Echo (ping) request  id=0xc93a, seq=1/256, ttl=64 (reply in 17)
     17 10.795202      10.2.2.2              10.2.2.3              ICMP     98     Echo (ping) reply    id=0xc93a, seq=1/256, ttl=64 (request in 16)
     18 11.402724      aa:bb:cc:00:03:00     Spanning-tree-(for-bridges)_00 STP      60     RST. Root = 32768/1/aa:bb:cc:00:02:00  Cost = 100  Port = 0x8001
     19 11.796147      10.2.2.3              10.2.2.2              ICMP     98     Echo (ping) request  id=0xca3a, seq=2/512, ttl=64 (reply in 20)
     20 11.796695      10.2.2.2              10.2.2.3              ICMP     98     Echo (ping) reply    id=0xca3a, seq=2/512, ttl=64 (request in 19)
     21 12.797275      10.2.2.3              10.2.2.2              ICMP     98     Echo (ping) request  id=0xcb3a, seq=3/768, ttl=64 (reply in 22)
     22 12.797754      10.2.2.2              10.2.2.3              ICMP     98     Echo (ping) reply    id=0xcb3a, seq=3/768, ttl=64 (request in 21)
     23 13.409361      aa:bb:cc:00:03:00     Spanning-tree-(for-bridges)_00 STP      60     RST. Root = 32768/1/aa:bb:cc:00:02:00  Cost = 100  Port = 0x8001
     24 15.413998      aa:bb:cc:00:03:00     Spanning-tree-(for-bridges)_00 STP      60     RST. Root = 32768/1/aa:bb:cc:00:02:00  Cost = 100  Port = 0x8001
     25 17.422080      aa:bb:cc:00:03:00     Spanning-tree-(for-bridges)_00 STP      60     RST. Root = 32768/1/aa:bb:cc:00:02:00  Cost = 100  Port = 0x8001
     26 19.434168      aa:bb:cc:00:03:00     Spanning-tree-(for-bridges)_00 STP      60     RST. Root = 32768/1/aa:bb:cc:00:02:00  Cost = 100  Port = 0x8001
     27 21.434755      aa:bb:cc:00:03:00     Spanning-tree-(for-bridges)_00 STP      60     RST. Root = 32768/1/aa:bb:cc:00:02:00  Cost = 100  Port = 0x8001
     28 23.446644      aa:bb:cc:00:03:00     Spanning-tree-(for-bridges)_00 STP      60     RST. Root = 32768/1/aa:bb:cc:00:02:00  Cost = 100  Port = 0x8001
     29 25.455226      aa:bb:cc:00:03:00     Spanning-tree-(for-bridges)_00 STP      60     RST. Root = 32768/1/aa:bb:cc:00:02:00  Cost = 100  Port = 0x8001

</code></pre>
<ol>
<li>PC3 envoi un message ARP who as 10.2.2.2 (PC2)</li>
<li>PC2 et PC1 reçoivent le message who as 10.2.2.2.</li>
<li>PC1 fait rien, il est pas concerné.</li>
<li>PC2 répond, en passant par un chemin défini grace au STP.</li>
<li>Le message ne passera pas par le port eth0/2 du switch1 car l’envoi à été bloqué par le STP.  <code>Et0/2 Altn BLK 100 128.3 Shr</code></li>
</ol>
<h4 id="reconfigurer-stp">Reconfigurer STP</h4>
<ul>
<li>🌞 changer la priorité d’un switch:</li>
</ul>
<pre><code>IOU-2#show spanning-tree br

                                                   Hello  Max  Fwd
Vlan                         Bridge ID              Time  Age  Dly  Protocol
---------------- --------------------------------- -----  ---  ---  --------
VLAN0001         32769 (32768,   1) aabb.cc00.0200    2    20   15  rstp        
IOU-2#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
IOU-2(config)#spanning-tree vlan 1 priority 16384
IOU-2(config)#exit
IOU-2#sho
*Oct 16 16:26:29.334: %SYS-5-CONFIG_I: Configured from console by console
IOU-2#show spanning-tree br

                                                   Hello  Max  Fwd
Vlan                         Bridge ID              Time  Age  Dly  Protocol
---------------- --------------------------------- -----  ---  ---  --------
VLAN0001         16385 (16384,   1) aabb.cc00.0200    2    20   15  rstp  
</code></pre>
<ul>
<li>🌞 vérifier les changements</li>
</ul>
<pre><code>IOU-2#show spanning-tree br

                                                  Hello  Max  Fwd
Vlan                         Bridge ID              Time  Age  Dly  Protocol
---------------- --------------------------------- -----  ---  ---  --------
VLAN0001         16385 (16384,   1) aabb.cc00.0200    2    20   15  rstp 
</code></pre>
<h1 id="iii.-isolation">III. Isolation</h1>
<h2 id="simple">1. Simple</h2>
<h4 id="topologie-2">Topologie</h4>
<pre><code>+-----+        +-------+        +-----+
| PC1 +--------+  SW1  +--------+ PC3 |
+-----+      10+-------+20      +-----+
                 20|
                   |
                +--+--+
                | PC2 |
                +-----+
</code></pre>
<p><img src="https://github.com/FlorianLeveil/RESEAU_B2/blob/master/Images/topo3.png" alt=""></p>
<h4 id="todo-1"><a href="#todo-2"></a>ToDo</h4>
<p>🌞 Mise en place la topologie ci-dessus avec des VLANs</p>
<ul>
<li>1 - On créer un Vlan avec la commande <code>vlan</code> suivie du numéro du vlan.</li>
<li>2 - Une fois le Vlan créée, on lui donne un nom <code>name client-network</code></li>
<li>3 - On passe l’interface en mode access puis on lui asigne le vlan 10</li>
</ul>
<pre><code>switchport mode access
witchport access vlan 10 
</code></pre>
<ul>
<li>🌞 faire communiquer les PCs deux à deux</li>
</ul>
<p>PC 2 vers PC3:</p>
<pre><code>PC--2&gt; ping 10.2.3.3
84 bytes from 10.2.3.3 icmp_seq=1 ttl=64 time=0.559 ms
84 bytes from 10.2.3.3 icmp_seq=2 ttl=64 time=0.247 ms
</code></pre>
<p>PC3 vers PC2:</p>
<pre><code>PC--3&gt; ping 10.2.3.2
84 bytes from 10.2.3.2 icmp_seq=1 ttl=64 time=0.346 ms
84 bytes from 10.2.3.2 icmp_seq=2 ttl=64 time=0.183 ms
</code></pre>
<p>PC1 vers PC2:</p>
<pre><code>PC--1 &gt; ping 10.2.3.2
host (10.2.3.2) not reachable
</code></pre>
<p>PC1 vers PC3</p>
<pre><code>PC--1&gt; ping 10.2.3.3
host (10.2.3.3) not reachable
</code></pre>
<h2 id="avec-trunk">2. Avec trunk</h2>
<h4 id="topologie-3">Topologie</h4>
<pre><code>+-----+        +-------+        +-------+        +-----+
| PC1 +--------+  SW1  +--------+  SW2  +--------+ PC4 |
+-----+      10+-------+        +-------+20      +-----+
                 20|              10|
                   |                |
                +--+--+          +--+--+
                | PC2 |          | PC3 |
                +-----+          +-----+
</code></pre>
<p><img src="https://github.com/FlorianLeveil/RESEAU_B2/blob/master/Images/topo4.png" alt=""></p>
<h4 id="todo-2">ToDo</h4>
<p>🌞 Mise en place la topologie ci-dessus :</p>
<ul>
<li>1 -On fait comme avant a part pour la jonction entre les deux switchs</li>
<li>2 - Pour la jonction entre les deux switch on passe les interface utilisé pour leurs communication en mode trunk <code>switchport mode trunk</code> .</li>
<li>3 - On défini l’encapsulation <code>switchport trunk encapsulation dot1q</code></li>
<li>4 - Et enfin on autorise les deux Vlan sur les ports des deux switchs <code>switchport trunk allowed vlan 10,20</code> !</li>
</ul>
<p>🌞 Faire communiquer les PCs deux à deux</p>
<ul>
<li>vérifier que <code>PC1</code> ne peut joindre que <code>PC3</code><br>
(Moi pour le coup c’est pc3 / pc4 / pc5 / pc6, j’ai juste zappé de les renommer)</li>
</ul>
<pre><code>PC3&gt; ping 10.2.10.3
84 bytes from 10.2.10.3 icmp_seq=2 ttl=64 time=0.385 ms
84 bytes from 10.2.10.3 icmp_seq=3 ttl=64 time=0.417 ms
</code></pre>
<pre><code>PC3&gt; ping 10.2.20.2
No gateway found
</code></pre>
<pre><code>PC3&gt; ping 10.2.20.4
No gateway found
</code></pre>
<ul>
<li>vérifier que <code>PC4</code> ne peut joindre que <code>PC2</code></li>
</ul>
<pre><code>PC6&gt; ping 10.2.10.2
84 bytes from 10.2.10.2 icmp_seq=3 ttl=64 time=0.398 ms
84 bytes from 10.2.10.2 icmp_seq=5 ttl=64 time=0.436 ms
</code></pre>
<pre><code>PC6&gt; ping 10.2.20.1
No gateway found
</code></pre>
<pre><code>PC6&gt; ping 10.2.20.3
No gateway found
</code></pre>
<h1 id="iv.-need-perfs">IV. Need perfs</h1>
<p>Mise en place d’un agrégat de port afin de bénéficier d’une meilleure performance ainsi que d’une meilleure sécurité.</p>
<h4 id="topologie-4">Topologie</h4>
<p>Pareil qu’en [III.2.] à part le lien entre SW1 et SW2 qui est doublé.</p>
<pre><code>+-----+        +-------+--------+-------+        +-----+
| PC1 +--------+  SW1  |        |  SW2  +--------+ PC4 |
+-----+      10+-------+--------+-------+20      +-----+
                 20|              10|
                   |                |
                +--+--+          +--+--+
                | PC2 |          | PC3 |
                +-----+          +-----+

</code></pre>
<h4 id="todo-3">ToDo</h4>
<ul>
<li>🌞 mettre en place la topologie ci-dessus
<ul>
<li>configurer LACP entre <code>SW1</code> et <code>SW2</code></li>
<li>utiliser Wireshark pour mettre en évidence l’utilisation de trames <a href="/it4lik/b2-reseau-2019/blob/master/memo/lexique.md#lacp-link-aggregation-control-protocol">LACP</a></li>
<li><strong>vérifier avec un <code>show ip interface po1</code> que la bande passante a bien été doublée</strong></li>
</ul>
</li>
</ul>

