---


---

<h1 id="i.-simplest-setup">I. Simplest setup</h1>
<p>Ci dessous nous pouvons constater les échanges ARP:</p>
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
<h4 id="topologie">Topologie</h4>
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
<ul>
<li>🌞 mettre en place la topologie ci-dessus</li>
</ul>
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
<li>🌞 analyser la table MAC d’un switch
<ul>
<li><code>show mac address-table</code></li>
<li>comprendre/expliquer chaque ligne</li>
</ul>
</li>
<li>🐙 en lançant Wireshark sur les liens des switches, il y a des trames CDP qui circulent. Quoi qu’est-ce ?</li>
</ul>
<h4 id="mise-en-évidence-du-spanning-tree-protocol"><a href="#mise-en-%C3%A9vidence-du-spanning-tree-protocol"></a>Mise en évidence du <a href="/it4lik/b2-reseau-2019/blob/master/memo/lexique.md#stp-spanning-tree-protocol">Spanning Tree Protocol</a></h4>
<p><a href="/it4lik/b2-reseau-2019/blob/master/memo/lexique.md#stp-spanning-tree-protocol">STP</a> a été ici automatiquement configuré par les switches eux-mêmes pour éviter une boucle réseau.</p>
<p>Dans une configuration pareille, les switches ont élu un chemin de préférence.<br>
Si on considère les trois liens qui unissent les switches :</p>
<ul>
<li><code>SW1</code> &lt;&gt; <code>SW2</code></li>
<li><code>SW2</code> &lt;&gt; <code>SW3</code></li>
<li><code>SW1</code> &lt;&gt; <code>SW3</code></li>
</ul>
<p><strong>L’un de ces liens a forcément été désactivé.</strong></p>
<p>On va regarder comment <a href="/it4lik/b2-reseau-2019/blob/master/memo/lexique.md#stp-spanning-tree-protocol">STP</a> a été configuré.</p>
<ul>
<li>🌞 déterminer les informations STP
<ul>
<li>à l’aide des <a href="/it4lik/b2-reseau-2019/blob/master/memo/cli-cisco.md#stp">commandes dédiées au protocole</a></li>
<li>qui est le <a href="/it4lik/b2-reseau-2019/blob/master/cours/1.md#overview-of-stp-behaviour"><em>root bridge</em></a>, quels sont les ports désactivés, etc.</li>
</ul>
</li>
<li>🌞 faire un schéma en représentant les informations STP
<ul>
<li>rôle des switches (qui est le root bridge)</li>
<li>rôle de chacun des ports</li>
</ul>
</li>
<li>🌞 confirmer les informations STP
<ul>
<li>effectuer un <code>ping</code> d’une machine à une autre</li>
<li>vérifier que les trames passent bien par le chemin attendu (Wireshark)</li>
</ul>
</li>
<li>🌞 ainsi, déterminer quel lien a été désactivé par STP</li>
<li>🌞 faire un schéma qui explique le trajet d’une requête ARP lorsque PC1 ping PC3, et de sa réponse
<ul>
<li>représenter <strong>TOUTES</strong> les trames ARP (n’oubliez pas les broadcasts)</li>
</ul>
</li>
</ul>
<h4 id="reconfigurer-stp"><a href="#reconfigurer-stp"></a>Reconfigurer STP</h4>
<ul>
<li>🌞 changer la priorité d’un switch qui n’est pas le <a href="/it4lik/b2-reseau-2019/blob/master/cours/1.md#overview-of-stp-behaviour"><em>root bridge</em></a></li>
<li>🌞 vérifier les changements
<ul>
<li>avec des commandes sur les switches</li>
<li>🐙 capturer les échanges qui suivent une reconfiguration STP avec Wireshark</li>
</ul>
</li>
</ul>
<h4 id="🐙-stp--perfs"><a href="#-stp-perfs"></a>🐙 STP &amp; Perfs</h4>
<p>Si vous avez lancé Wireshark sur un lien entre un PC et un Switch, vous avez vu qu’il y a toujours des trames STP qui circulent…</p>
<ul>
<li>un peu con non ? C’est un PC, il enverra jamais de trames STP</li>
<li>aussi avec STP, quand on branche un PC, le lien mettra plusieurs secondes avant de passer en <em>forwarding</em> et ainsi transmettre de la donnée</li>
<li>l’idéal ça serait de désactiver l’envoi de trames STP sur l’interface du switch (ça évite de cramer de la bande passante et du calcul CPU pour rien, générer du trafic inutile, etc.)</li>
<li>sauuuuf que si un p’tit malin branche des switches là-dessus, il pourrait tout péter en créant une boucle</li>
<li>deux fonctionnalités à mettre en place :
<ul>
<li><code>portfast</code> : marque un port comme <em>“edge”</em> dans la topologie STP. Un port <em>edge</em> est considéré comme une extrémité de la topologie (= un client branché dessus, port <em>access</em>). <em>Port<strong>fast</strong></em> parce que ça va permettre au port de s’allumer plus rapidement (sans passer par les états <em>listening</em> et <em>learning</em> pendant 15 secondes chacun par défaut) et d’être disponible instantanément
<ul>
<li>on peut voir l’état d’un port (forward, listening, learning, blocking avec <code>show spanning-tree vlan 1</code>)</li>
</ul>
</li>
<li><code>bpduguard</code> : permet de shutdown le port s’il reçoit des <em>BPDU</em> (pour rappel : un <em>BPDU</em> c’est un message STP)</li>
</ul>
</li>
</ul>
<p>Idem pour les trames CDP !</p>
<p>🐙 ToDo :</p>
<ul>
<li><a href="/it4lik/b2-reseau-2019/blob/master/memo/cli-cisco.md#stp">activer ces fonctionnalités (<em>portfast</em> et <em>bpduguard</em>) et activer le filtre BPDU</a> sur les interfaces où c’est nécessaire (marqué comme <em>edge</em> dans la topologie STP)</li>
<li>aussi <a href="/it4lik/b2-reseau-2019/blob/master/memo/cli-cisco.md#cdp">désactiver l’envoi de trames CDP</a> sur ces ports
<ul>
<li>prouver avec Wireshark que le switch n’envoie plus de BPDU ni de trames CDP</li>
<li>faites une capture <strong>avant</strong> et une capture <strong>après</strong> les manips pour le prouver :)</li>
</ul>
</li>
</ul>
<h1 id="iii.-isolation"><a href="#iii-isolation"></a>III. Isolation</h1>
<p>Ici on va s’intéresser à l’utilisation de <a href="#vlan-virtual-local-area-network">VLANs</a>.</p>
<h2 id="simple"><a href="#1-simple"></a>1. Simple</h2>
<h4 id="topologie-1"><a href="#topologie-2"></a>Topologie</h4>
<pre><code>+-----+        +-------+        +-----+
| PC1 +--------+  SW1  +--------+ PC3 |
+-----+      10+-------+20      +-----+
                 20|
                   |
                +--+--+
                | PC2 |
                +-----+
</code></pre>
<h4 id="plan-dadressage"><a href="#plan-dadressage-2"></a>Plan d’adressage</h4>
<p>Machine</p>
<p>IP <code>net1</code></p>
<p>VLAN</p>
<p><code>PC1</code></p>
<p><code>10.2.3.1/24</code></p>
<p>10</p>
<p><code>PC2</code></p>
<p><code>10.2.3.2/24</code></p>
<p>20</p>
<p><code>PC3</code></p>
<p><code>10.2.3.3/24</code></p>
<p>20</p>
<h4 id="todo-1"><a href="#todo-2"></a>ToDo</h4>
<ul>
<li>🌞 mettre en place la topologie ci-dessus avec des <a href="#vlan-virtual-local-area-network">VLANs</a>
<ul>
<li>voir <a href="/it4lik/b2-reseau-2019/blob/master/memo/cli-cisco.md#vlan">les commandes dédiées à la manipulation de VLANs</a></li>
</ul>
</li>
<li>🌞 faire communiquer les PCs deux à deux
<ul>
<li>vérifier que <code>PC2</code> ne peut joindre que <code>PC3</code></li>
<li>vérifier que <code>PC1</code> ne peut joindre personne alors qu’il est dans le même réseau (<strong>sad</strong>)</li>
</ul>
</li>
</ul>
<h2 id="avec-trunk"><a href="#2-avec-trunk"></a>2. Avec trunk</h2>
<h4 id="topologie-2"><a href="#topologie-3"></a>Topologie</h4>
<pre><code>+-----+        +-------+        +-------+        +-----+
| PC1 +--------+  SW1  +--------+  SW2  +--------+ PC4 |
+-----+      10+-------+        +-------+20      +-----+
                 20|              10|
                   |                |
                +--+--+          +--+--+
                | PC2 |          | PC3 |
                +-----+          +-----+
</code></pre>
<h4 id="plan-dadressage-1"><a href="#plan-dadressage-3"></a>Plan d’adressage</h4>
<p>Machine</p>
<p>IP <code>net1</code></p>
<p>IP <code>net2</code></p>
<p>VLAN</p>
<p><code>PC1</code></p>
<p><code>10.2.10.1/24</code></p>
<p>X</p>
<p>10</p>
<p><code>PC2</code></p>
<p>X</p>
<p><code>10.2.20.1/24</code></p>
<p>20</p>
<p><code>PC3</code></p>
<p><code>10.2.10.2/24</code></p>
<p>X</p>
<p>10</p>
<p><code>PC4</code></p>
<p>X</p>
<p><code>10.2.20.2/24</code></p>
<p>20</p>
<h4 id="todo-2"><a href="#todo-3"></a>ToDo</h4>
<ul>
<li>🌞 mettre en place la topologie ci-dessus (en utilisant des <a href="#vlan-virtual-local-area-network">VLAN</a>, vous aurez besoin de <a href="/it4lik/b2-reseau-2019/blob/master/cours/1.md#cisco-ports-access-et-trunk">la notion de <em>trunk</em></a>)</li>
<li>🌞 faire communiquer les PCs deux à deux
<ul>
<li>vérifier que <code>PC1</code> ne peut joindre que <code>PC3</code></li>
<li>vérifier que <code>PC4</code> ne peut joindre que <code>PC2</code></li>
</ul>
</li>
<li>🌞 mettre en évidence l’utilisation des VLANs avec Wireshark</li>
</ul>
<h1 id="iv.-need-perfs"><a href="#iv-need-perfs"></a>IV. Need perfs</h1>
<p>Mise en place d’un agrégat de port afin de bénéficier d’une meilleure performance ainsi que d’une meilleure sécurité.</p>
<h4 id="topologie-3"><a href="#topologie-4"></a>Topologie</h4>
<p>Pareil qu’en <a href="#2-avec-trunk">III.2.</a> à part le lien entre SW1 et SW2 qui est doublé.</p>
<pre><code>+-----+        +-------+--------+-------+        +-----+
| PC1 +--------+  SW1  |        |  SW2  +--------+ PC4 |
+-----+      10+-------+--------+-------+20      +-----+
                 20|              10|
                   |                |
                +--+--+          +--+--+
                | PC2 |          | PC3 |
                +-----+          +-----+

</code></pre>
<h4 id="plan-dadressage-2"><a href="#plan-dadressage-4"></a>Plan d’adressage</h4>
<p>Pareil qu’en <a href="#2-avec-trunk">III.2.</a>.</p>
<p>Machine</p>
<p>IP <code>net1</code></p>
<p>IP <code>net2</code></p>
<p>VLAN</p>
<p><code>PC1</code></p>
<p><code>10.2.10.1/24</code></p>
<p>X</p>
<p>10</p>
<p><code>PC2</code></p>
<p>X</p>
<p><code>10.2.20.1/24</code></p>
<p>20</p>
<p><code>PC3</code></p>
<p><code>10.2.10.2/24</code></p>
<p>X</p>
<p>10</p>
<p><code>PC4</code></p>
<p>X</p>
<p><code>10.2.20.2/24</code></p>
<p>20</p>
<h4 id="todo-3"><a href="#todo-4"></a>ToDo</h4>
<ul>
<li>🌞 mettre en place la topologie ci-dessus
<ul>
<li>configurer LACP entre <code>SW1</code> et <code>SW2</code></li>
<li>utiliser Wireshark pour mettre en évidence l’utilisation de trames <a href="/it4lik/b2-reseau-2019/blob/master/memo/lexique.md#lacp-link-aggregation-control-protocol">LACP</a></li>
<li><strong>vérifier avec un <code>show ip interface po1</code> que la bande passante a bien été doublée</strong></li>
</ul>
</li>
</ul>
<blockquote>
<p>Pas de failover possible sur les IOUs malheureusement :( (voir <a href="https://www.cisco.com/c/en/us/td/docs/switches/blades/3020/software/release/12-2_58_se/configuration/guide/3020_scg/swethchl.pdf">ce doc</a>, dernière section. Pas de <code>link state</code> dans les IOUs)</p>
</blockquote>

