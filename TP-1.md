# TP1
## I. Gather informations

Récupéreration de la liste des cartes réseau:
```
[florian@localhost ~]$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:52:03:ef brd ff:ff:ff:ff:ff:ff
    inet 10.0.2.15/24 brd 10.0.2.255 scope global dynamic noprefixroute enp0s3
       valid_lft 86083sec preferred_lft 86083sec
    inet6 fe80::fd43:f340:e369:ff69/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:c9:40:c1 brd ff:ff:ff:ff:ff:ff
    inet 192.168.57.3/24 brd 192.168.57.255 scope global dynamic noprefixroute enp0s8
       valid_lft 883sec preferred_lft 883sec
    inet6 fe80::a00:27ff:fec9:40c1/64 scope link 
       valid_lft forever preferred_lft forever
```

Déterminer si les cartes réseaux ont récupéré une IP en DHCP:
* enp0s3: (oui)
```
[florian@localhost ~]$ cat /etc/sysconfig/network-scripts/ifcfg-enp0s3
TYPE="Ethernet"
PROXY_METHOD="none"
BROWSER_ONLY="no"
BOOTPROTO="dhcp"
DEFROUTE="yes"
IPV4_FAILURE_FATAL="no"
IPV6INIT="yes"
IPV6_AUTOCONF="yes"
IPV6_DEFROUTE="yes"
IPV6_FAILURE_FATAL="no"
IPV6_ADDR_GEN_MODE="stable-privacy"
NAME="enp0s3"
UUID="029db16f-01fc-42fe-8a06-428d5801b374"
DEVICE="enp0s3"
ONBOOT="yes"

```

* enps0s8: (oui)
```
[florian@localhost ~]$ cat /etc/sysconfig/network-scripts/ifcfg-enp0s8
TYPE="Ethernet"
BOOTPROTO="dhcp"

NAME="enp0s8"
DEVICE="enp0s8"
ONBOOT="yes"

```

Bail DHCP:
* enps0s3:
```
DHCP4.OPTION[1]:                        domain_name = auvence.co
DHCP4.OPTION[2]:                        domain_name_servers = 10.33.10.20 10.33.10.2 8.8.8.8
DHCP4.OPTION[3]:                        expiry = 1569572141
DHCP4.OPTION[4]:                        ip_address = 10.0.2.15
DHCP4.OPTION[5]:                        requested_broadcast_address = 1
DHCP4.OPTION[6]:                        requested_dhcp_server_identifier = 1
DHCP4.OPTION[7]:                        requested_domain_name = 1
DHCP4.OPTION[8]:                        requested_domain_name_servers = 1
DHCP4.OPTION[9]:                        requested_domain_search = 1
DHCP4.OPTION[10]:                       requested_host_name = 1
DHCP4.OPTION[11]:                       requested_interface_mtu = 1
DHCP4.OPTION[12]:                       requested_ms_classless_static_routes = 1
DHCP4.OPTION[13]:                       requested_nis_domain = 1
DHCP4.OPTION[14]:                       requested_nis_servers = 1
DHCP4.OPTION[15]:                       requested_ntp_servers = 1
DHCP4.OPTION[16]:                       requested_rfc3442_classless_static_routes = 1
DHCP4.OPTION[17]:                       requested_routers = 1
DHCP4.OPTION[18]:                       requested_static_routes = 1
DHCP4.OPTION[19]:                       requested_subnet_mask = 1
DHCP4.OPTION[20]:                       requested_time_offset = 1
DHCP4.OPTION[21]:                       requested_wpad = 1
DHCP4.OPTION[22]:                       routers = 10.0.2.2
DHCP4.OPTION[23]:                       subnet_mask = 255.255.255.0

```
* enp0s8:
```
[florian@localhost ~]$ sudo nmcli -f DHCP4 con show enp0s8
DHCP4.OPTION[1]:                        expiry = 1569489341
DHCP4.OPTION[2]:                        ip_address = 192.168.57.3
DHCP4.OPTION[3]:                        requested_broadcast_address = 1
DHCP4.OPTION[4]:                        requested_dhcp_server_identifier = 1
DHCP4.OPTION[5]:                        requested_domain_name = 1
DHCP4.OPTION[6]:                        requested_domain_name_servers = 1
DHCP4.OPTION[7]:                        requested_domain_search = 1
DHCP4.OPTION[8]:                        requested_host_name = 1
DHCP4.OPTION[9]:                        requested_interface_mtu = 1
DHCP4.OPTION[10]:                       requested_ms_classless_static_routes = 1
DHCP4.OPTION[11]:                       requested_nis_domain = 1
DHCP4.OPTION[12]:                       requested_nis_servers = 1
DHCP4.OPTION[13]:                       requested_ntp_servers = 1
DHCP4.OPTION[14]:                       requested_rfc3442_classless_static_routes = 1
DHCP4.OPTION[15]:                       requested_routers = 1
DHCP4.OPTION[16]:                       requested_static_routes = 1
DHCP4.OPTION[17]:                       requested_subnet_mask = 1
DHCP4.OPTION[18]:                       requested_time_offset = 1
DHCP4.OPTION[19]:                       requested_wpad = 1
DHCP4.OPTION[20]:                       subnet_mask = 255.255.255.0

```


Table de routage de la machine et sa table ARP:
```
[florian@localhost ~]$ ip route
default via 10.0.2.2 dev enp0s3 proto dhcp metric 100 
10.0.2.0/24 dev enp0s3 proto kernel scope link src 10.0.2.15 metric 100 
192.168.57.0/24 dev enp0s8 proto kernel scope link src 192.168.57.3 metric 101

```
- Carte enp0s3, elle est utilisée pour une connexion externe, la passerelle de cette route est à l'IP 10.0.2.2
- Carte enp0s8, elle est utilisée pour une connexion local.
```
[florian@localhost ~]$ ip n s
192.168.57.1 dev enp0s8 lladdr 0a:00:27:00:00:01 REACHABLE
10.0.2.2 dev enp0s3 lladdr 52:54:00:12:35:02 STALE
192.168.57.2 dev enp0s8 lladdr 08:00:27:3a:ed:1a STALE
```
* 192.168.54.1 = IP de mon hote en local
* 10.0.2.2 = IP de mon hote en externe, ma passerelle
* 192.168.57.2 = IP d'un truc qui tourne sur mon pc mais je sais pas trop quoi !


Récupérer la liste des ports en écoute:
```
[florian@localhost ~]$ ss -ltu
Netid  State    Recv-Q    Send-Q              Local Address:Port         Peer Address:Port   
udp    UNCONN   0         0             192.168.57.3%enp0s8:bootpc            0.0.0.0:*      
udp    UNCONN   0         0                10.0.2.15%enp0s3:bootpc            0.0.0.0:*      
udp    UNCONN   0         0                       127.0.0.1:323               0.0.0.0:*      
udp    UNCONN   0         0                           [::1]:323                  [::]:*      
tcp    LISTEN   0         128                       0.0.0.0:ssh               0.0.0.0:*      
tcp    LISTEN   0         128                          [::]:ssh                  [::]:* 
```

Trouver/déduire la liste des applications qui écoutent sur un port TCP ou UDP sur la machine (au moins un serveur SSH).
```
[florian@localhost ~]$ ss -pn
Netid State  Recv-Q  Send-Q                                             Local Address:Port        Peer Address:Port                                                                       
u_str ESTAB  0       0                                    /run/dbus/system_bus_socket 21655                  * 21654                                                                      
u_str ESTAB  0       0                        /var/lib/sss/pipes/private/sbus-monitor 21473                  * 21472                                                                      
u_str ESTAB  0       0                                    /run/systemd/journal/stdout 18623                  * 18622                                                                      
u_str ESTAB  0       0                                                              * 28464                  * 28465      users:(("systemd",pid=1518,fd=19))                              
u_str ESTAB  0       0                                                              * 20781                  * 20780                                                                      
u_str ESTAB  0       0                                    /run/systemd/journal/stdout 21218                  * 21217                                                                      
u_str ESTAB  0       0                                    /run/dbus/system_bus_socket 21318                  * 20941                                                                      
u_str ESTAB  0       0                                                              * 28177                  * 0                                                                          
u_str ESTAB  0       0                                                              * 22687                  * 0                                                                          
u_str ESTAB  0       0                                                              * 27039                  * 0                                                                          
u_str ESTAB  0       0                                                              * 28670                  * 0                                                                          
u_str ESTAB  0       0                                                              * 20992                  * 20993                                                                      
u_str ESTAB  0       0                                                              * 28400                  * 28402      users:(("systemd",pid=1518,fd=2),("systemd",pid=1518,fd=1))     
u_str ESTAB  0       0                                                              * 21571                  * 21572                                                                      
u_str ESTAB  0       0                                    /run/systemd/journal/stdout 20993                  * 20992                                                                      
u_str ESTAB  0       0                                                              * 28621                  * 0                                                                          
u_str ESTAB  0       0                                                              * 27041                  * 27042                                                                      
u_str ESTAB  0       0                                                              * 23710                  * 23711                                                                      
u_str ESTAB  0       0                                                              * 21767                  * 21768                                                                      
u_str ESTAB  0       0                                                              * 21472                  * 21473                                                                      
u_str ESTAB  0       0                                                              * 22350                  * 0                                                                          
u_str ESTAB  0       0                                                              * 21448                  * 21449                                                                      
u_str ESTAB  0       0          /var/lib/sss/pipes/private/sbus-dp_implicit_files.760 21572                  * 21571                                                                      
u_str ESTAB  0       0                                    /run/systemd/journal/stdout 22075                  * 22074                                                                      
u_str ESTAB  0       0                                                              * 21639                  * 21640                                                                      
u_str ESTAB  0       0                        /var/lib/sss/pipes/private/sbus-monitor 21579                  * 21578                                                                      
u_str ESTAB  0       0                                                              * 21578                  * 21579                                                                      
u_str ESTAB  0       0                                                              * 21953                  * 0                                                                          
u_str ESTAB  0       0                                    /run/systemd/journal/stdout 22562                  * 22561                                                                      
u_str ESTAB  0       0                                                              * 21317                  * 21316                                                                      
u_str ESTAB  0       0                                    /run/dbus/system_bus_socket 21926                  * 21925                                                                      
u_str ESTAB  0       0                                                              * 22561                  * 22562                                                                      
u_str ESTAB  0       0                                                              * 22906                  * 22907                                                                      
u_str ESTAB  0       0                                                              * 28466                  * 0                                                                          
u_str ESTAB  0       0                                    /run/dbus/system_bus_socket 23711                  * 23710                                                                      
u_str ESTAB  0       0                                    /run/dbus/system_bus_socket 21768                  * 21767                                                                      
u_str ESTAB  0       0                                                              * 22144                  * 22145                                                                      
u_str ESTAB  0       0                                    /run/systemd/journal/stdout 28402                  * 28400                                                                      
u_str ESTAB  0       0                                                              * 33669                  * 0                                                                          
u_str ESTAB  0       0                                                              * 21217                  * 21218                                                                      
u_str ESTAB  0       0                                                              * 21548                  * 21549                                                                      
u_str ESTAB  0       0                                                              * 24303                  * 24302                                                                      
u_str ESTAB  0       0                                                              * 28433                  * 0                                                                          
u_str ESTAB  0       0                        /var/lib/sss/pipes/private/sbus-monitor 21557                  * 21556                                                                      
u_str ESTAB  0       0                                    /run/dbus/system_bus_socket 21449                  * 21448                                                                      
u_str ESTAB  0       0                                                              * 21654                  * 21655                                                                      
u_str ESTAB  0       0                                                              * 28690                  * 28691                                                                      
u_str ESTAB  0       0                                                              * 27043                  * 0                                                                          
u_str ESTAB  0       0                                    /run/systemd/journal/stdout 22145                  * 22144                                                                      
u_str ESTAB  0       0                                                              * 18622                  * 18623                                                                      
u_str ESTAB  0       0                                    /run/dbus/system_bus_socket 28465                  * 28464                                                                      
u_str ESTAB  0       0                                                              * 28691                  * 28690                                                                      
u_str ESTAB  0       0                                                              * 27042                  * 27041                                                                      
u_str ESTAB  0       0                                                              * 20780                  * 20781                                                                      
u_str ESTAB  0       0                                    /run/systemd/journal/stdout 21163                  * 21162                                                                      
u_str ESTAB  0       0                                                              * 20941                  * 21318                                                                      
u_str ESTAB  0       0                                                              * 21556                  * 21557                                                                      
u_str ESTAB  0       0          /var/lib/sss/pipes/private/sbus-dp_implicit_files.760 21549                  * 21548                                                                      
u_str ESTAB  0       0                                                              * 21162                  * 21163                                                                      
u_str ESTAB  0       0                                                              * 24302                  * 24303                                                                      
u_str ESTAB  0       0                                                              * 22074                  * 22075                                                                      
u_str ESTAB  0       0                                    /run/systemd/journal/stdout 21640                  * 21639                                                                      
u_str ESTAB  0       0                                                              * 21316                  * 21317                                                                      
u_str ESTAB  0       0                                    /run/systemd/journal/stdout 21832                  * 21831                                                                      
u_str ESTAB  0       0                                                              * 21925                  * 21926                                                                      
u_str ESTAB  0       0                                                              * 21831                  * 21832                                                                      
u_str ESTAB  0       0                                    /run/dbus/system_bus_socket 22907                  * 22906                                                                      
tcp   ESTAB  0       0                                                   192.168.57.3:22          192.168.57.1:39800    
```



Récupérer la liste des DNS utilisés par la machine:
```
[florian@localhost ~]$ nmcli device show enp0s3
GENERAL.DEVICE:                         enp0s3
GENERAL.TYPE:                           ethernet
GENERAL.HWADDR:                         08:00:27:52:03:EF
GENERAL.MTU:                            1500
GENERAL.STATE:                          100 (connected)
GENERAL.CONNECTION:                     enp0s3
GENERAL.CON-PATH:                       /org/freedesktop/NetworkManager/ActiveConnection/1
WIRED-PROPERTIES.CARRIER:               on
IP4.ADDRESS[1]:                         10.0.2.15/24
IP4.GATEWAY:                            10.0.2.2
IP4.ROUTE[1]:                           dst = 0.0.0.0/0, nh = 10.0.2.2, mt = 100
IP4.ROUTE[2]:                           dst = 10.0.2.0/24, nh = 0.0.0.0, mt = 100
IP4.DNS[1]:                             10.33.10.20
IP4.DNS[2]:                             10.33.10.2
IP4.DNS[3]:                             8.8.8.8
IP4.DOMAIN[1]:                          auvence.co
IP6.ADDRESS[1]:                         fe80::fd43:f340:e369:ff69/64
IP6.GATEWAY:                            --
IP6.ROUTE[1]:                           dst = fe80::/64, nh = ::, mt = 100
IP6.ROUTE[2]:                           dst = ff00::/8, nh = ::, mt = 256, table=255
```



Requête DNS afin de récupérer l'adresse IP associée au domaine www.reddit.com:

```
[florian@localhost ~]$ dig www.regit.com

; <<>> DiG 9.11.4-P2-RedHat-9.11.4-17.P2.el8_0.1 <<>> www.regit.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 45061
;; flags: qr rd ra; QUERY: 1, ANSWER: 2, AUTHORITY: 13, ADDITIONAL: 27

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;www.regit.com.			IN	A

;; ANSWER SECTION:
www.regit.com.		3599	IN	CNAME	regit.com.
regit.com.		599	IN	A	184.168.131.241

;; AUTHORITY SECTION:
com.			164654	IN	NS	i.gtld-servers.net.
com.			164654	IN	NS	j.gtld-servers.net.
com.			164654	IN	NS	g.gtld-servers.net.
com.			164654	IN	NS	b.gtld-servers.net.
com.			164654	IN	NS	l.gtld-servers.net.
com.			164654	IN	NS	h.gtld-servers.net.
com.			164654	IN	NS	a.gtld-servers.net.
com.			164654	IN	NS	m.gtld-servers.net.
com.			164654	IN	NS	k.gtld-servers.net.
com.			164654	IN	NS	e.gtld-servers.net.
com.			164654	IN	NS	d.gtld-servers.net.
com.			164654	IN	NS	f.gtld-servers.net.
com.			164654	IN	NS	c.gtld-servers.net.

;; ADDITIONAL SECTION:
a.gtld-servers.net.	164654	IN	A	192.5.6.30
a.gtld-servers.net.	164654	IN	AAAA	2001:503:a83e::2:30
b.gtld-servers.net.	164654	IN	A	192.33.14.30
b.gtld-servers.net.	164654	IN	AAAA	2001:503:231d::2:30
c.gtld-servers.net.	164654	IN	A	192.26.92.30
c.gtld-servers.net.	164654	IN	AAAA	2001:503:83eb::30
d.gtld-servers.net.	164654	IN	A	192.31.80.30
d.gtld-servers.net.	164654	IN	AAAA	2001:500:856e::30
e.gtld-servers.net.	164654	IN	A	192.12.94.30
e.gtld-servers.net.	164654	IN	AAAA	2001:502:1ca1::30
f.gtld-servers.net.	164654	IN	A	192.35.51.30
f.gtld-servers.net.	164654	IN	AAAA	2001:503:d414::30
g.gtld-servers.net.	164654	IN	A	192.42.93.30
g.gtld-servers.net.	164654	IN	AAAA	2001:503:eea3::30
h.gtld-servers.net.	164654	IN	A	192.54.112.30
h.gtld-servers.net.	164654	IN	AAAA	2001:502:8cc::30
i.gtld-servers.net.	164654	IN	A	192.43.172.30
i.gtld-servers.net.	164654	IN	AAAA	2001:503:39c1::30
j.gtld-servers.net.	164654	IN	A	192.48.79.30
j.gtld-servers.net.	164654	IN	AAAA	2001:502:7094::30
k.gtld-servers.net.	164654	IN	A	192.52.178.30
k.gtld-servers.net.	164654	IN	AAAA	2001:503:d2d::30
l.gtld-servers.net.	164654	IN	A	192.41.162.30
l.gtld-servers.net.	164654	IN	AAAA	2001:500:d937::30
m.gtld-servers.net.	164654	IN	A	192.55.83.30
m.gtld-servers.net.	164654	IN	AAAA	2001:501:b1f9::30

;; Query time: 195 msec
;; SERVER: 10.33.10.20#53(10.33.10.20)
;; WHEN: Thu Sep 26 06:41:37 EDT 2019
;; MSG SIZE  rcvd: 868


```

Afficher l'état actuel du firewall:
```
[florian@localhost ~]$ systemctl status firewalld.service
● firewalld.service - firewalld - dynamic firewall daemon
   Loaded: loaded (/usr/lib/systemd/system/firewalld.service; enabled; vendor preset: enable>
   Active: active (running) since Thu 2019-09-26 04:15:36 EDT; 2h 29min ago
     Docs: man:firewalld(1)
 Main PID: 763 (firewalld)
    Tasks: 2 (limit: 11513)
   Memory: 35.7M
   CGroup: /system.slice/firewalld.service
           └─763 /usr/libexec/platform-python -s /usr/sbin/firewalld --nofork --nopid

Sep 26 04:15:35 localhost.localdomain systemd[1]: Starting firewalld - dynamic firewall daem>
Sep 26 04:15:36 localhost.localdomain systemd[1]: Started firewalld - dynamic firewall daemo>

```

Interfaces filtrées:
```
[florian@localhost ~]$ firewall-cmd --get-active-zones
public
  interfaces: enp0s3 enp0s8
```
Port TCP/UDP sont autorisés/filtrés:
```
[florian@localhost ~]$ sudo firewall-cmd --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: enp0s3 enp0s8
  sources: 
  services: cockpit dhcpv6-client ssh
  ports: 
  protocols: 
  masquerade: no
  forward-ports: 
  source-ports: 
  icmp-blocks: 
  rich rules: 
```
🐙 nft:

```
[florian@localhost ~]$ sudo nft list tables
table ip filter
table ip6 filter
table bridge filter
table ip security
table ip raw
table ip mangle
table ip nat
table ip6 security
table ip6 raw
table ip6 mangle
table ip6 nat
table bridge nat
table inet firewalld
table ip firewalld
table ip6 firewalld
```
```
[florian@localhost ~]$ sudo nft list table inet firewalld
table inet firewalld {
	chain raw_PREROUTING {
		type filter hook prerouting priority -290; policy accept;
		icmpv6 type { nd-router-advert, nd-neighbor-solicit } accept
		meta nfproto ipv6 fib saddr . iif oif missing drop
		jump raw_PREROUTING_ZONES_SOURCE
		jump raw_PREROUTING_ZONES
	}

	chain raw_PREROUTING_ZONES_SOURCE {
	}

	chain raw_PREROUTING_ZONES {
		iifname "enp0s8" goto raw_PRE_public
		iifname "enp0s3" goto raw_PRE_public
		goto raw_PRE_public
	}

	chain mangle_PREROUTING {
		type filter hook prerouting priority -140; policy accept;
		jump mangle_PREROUTING_ZONES_SOURCE
		jump mangle_PREROUTING_ZONES
	}

	chain mangle_PREROUTING_ZONES_SOURCE {
	}

	chain mangle_PREROUTING_ZONES {
		iifname "enp0s8" goto mangle_PRE_public
		iifname "enp0s3" goto mangle_PRE_public
		goto mangle_PRE_public
	}

	chain filter_INPUT {
		type filter hook input priority 10; policy accept;
		ct state established,related accept
		iifname "lo" accept
		jump filter_INPUT_ZONES_SOURCE
		jump filter_INPUT_ZONES
		ct state invalid drop
		reject with icmpx type admin-prohibited
	}

	chain filter_FORWARD {
		type filter hook forward priority 10; policy accept;
		ct state established,related accept
		iifname "lo" accept
		ip6 daddr { ::/96, ::ffff:0.0.0.0/96, 2002::/24, 2002:a00::/24, 2002:7f00::/24, 2002:a9fe::/32, 2002:ac10::/28, 2002:c0a8::/32, 2002:e000::/19 } reject with icmpv6 type addr-unreachable
		jump filter_FORWARD_IN_ZONES_SOURCE
		jump filter_FORWARD_IN_ZONES
		jump filter_FORWARD_OUT_ZONES_SOURCE
		jump filter_FORWARD_OUT_ZONES
		ct state invalid drop
		reject with icmpx type admin-prohibited
	}

	chain filter_OUTPUT {
		type filter hook output priority 10; policy accept;
		oifname "lo" accept
		ip6 daddr { ::/96, ::ffff:0.0.0.0/96, 2002::/24, 2002:a00::/24, 2002:7f00::/24, 2002:a9fe::/32, 2002:ac10::/28, 2002:c0a8::/32, 2002:e000::/19 } reject with icmpv6 type addr-unreachable
	}

	chain filter_INPUT_ZONES_SOURCE {
	}

	chain filter_INPUT_ZONES {
		iifname "enp0s8" goto filter_IN_public
		iifname "enp0s3" goto filter_IN_public
		goto filter_IN_public
	}

	chain filter_FORWARD_IN_ZONES_SOURCE {
	}

	chain filter_FORWARD_IN_ZONES {
		iifname "enp0s8" goto filter_FWDI_public
		iifname "enp0s3" goto filter_FWDI_public
		goto filter_FWDI_public
	}

	chain filter_FORWARD_OUT_ZONES_SOURCE {
	}

	chain filter_FORWARD_OUT_ZONES {
		oifname "enp0s8" goto filter_FWDO_public
		oifname "enp0s3" goto filter_FWDO_public
		goto filter_FWDO_public
	}

	chain raw_PRE_public {
		jump raw_PRE_public_pre
		jump raw_PRE_public_log
		jump raw_PRE_public_deny
		jump raw_PRE_public_allow
		jump raw_PRE_public_post
	}

	chain raw_PRE_public_pre {
	}

	chain raw_PRE_public_log {
	}

	chain raw_PRE_public_deny {
	}

	chain raw_PRE_public_allow {
	}

	chain raw_PRE_public_post {
	}

	chain filter_IN_public {
		jump filter_IN_public_pre
		jump filter_IN_public_log
		jump filter_IN_public_deny
		jump filter_IN_public_allow
		jump filter_IN_public_post
		meta l4proto { icmp, ipv6-icmp } accept
	}

	chain filter_IN_public_pre {
	}

	chain filter_IN_public_log {
	}

	chain filter_IN_public_deny {
	}

	chain filter_IN_public_allow {
		tcp dport ssh ct state new,untracked accept
		ip6 daddr fe80::/64 udp dport dhcpv6-client ct state new,untracked accept
		tcp dport 9090 ct state new,untracked accept
	}

	chain filter_IN_public_post {
	}

	chain filter_FWDI_public {
		jump filter_FWDI_public_pre
		jump filter_FWDI_public_log
		jump filter_FWDI_public_deny
		jump filter_FWDI_public_allow
		jump filter_FWDI_public_post
		meta l4proto { icmp, ipv6-icmp } accept
	}

	chain filter_FWDI_public_pre {
	}

	chain filter_FWDI_public_log {
	}

	chain filter_FWDI_public_deny {
	}

	chain filter_FWDI_public_allow {
	}

	chain filter_FWDI_public_post {
	}

	chain mangle_PRE_public {
		jump mangle_PRE_public_pre
		jump mangle_PRE_public_log
		jump mangle_PRE_public_deny
		jump mangle_PRE_public_allow
		jump mangle_PRE_public_post
	}

	chain mangle_PRE_public_pre {
	}

	chain mangle_PRE_public_log {
	}

	chain mangle_PRE_public_deny {
	}

	chain mangle_PRE_public_allow {
	}

	chain mangle_PRE_public_post {
	}

	chain filter_FWDO_public {
		jump filter_FWDO_public_pre
		jump filter_FWDO_public_log
		jump filter_FWDO_public_deny
		jump filter_FWDO_public_allow
		jump filter_FWDO_public_post
	}

	chain filter_FWDO_public_pre {
	}

	chain filter_FWDO_public_log {
	}

	chain filter_FWDO_public_deny {
	}

	chain filter_FWDO_public_allow {
	}

	chain filter_FWDO_public_post {
	}
}
```

## II. Edit configuration
### 1. Configuration cartes réseau

Modification de la configuration de la carte réseau privée pour avoir une nouvelle IP statique définie par vos soins:

```
[florian@localhost network-scripts]$ cat ./ifcfg-enp0s8
TYPE="Ethernet"
BOOTPROTO="static"

NAME="enp0s8"
DEVICE="enp0s8"
ONBOOT="yes"

IPADDR="192.168.57.3"
MASK="255.255.255.0"
```
```
[florian@localhost network-scripts]$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:52:03:ef brd ff:ff:ff:ff:ff:ff
    inet 10.0.2.15/24 brd 10.0.2.255 scope global dynamic noprefixroute enp0s3
       valid_lft 84628sec preferred_lft 84628sec
    inet6 fe80::fd43:f340:e369:ff69/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:c9:40:c1 brd ff:ff:ff:ff:ff:ff
    inet 192.168.57.3/24 brd 192.168.57.255 scope global noprefixroute enp0s8
       valid_lft forever preferred_lft forever
```


Ajouter une nouvelle carte réseau dans un DEUXIEME réseau privé
```
[florian@localhost network-scripts]$ cat ./ifcfg-enp0s9
TYPE="Ethernet"
BOOTPROTO="static"

NAME="enp0s9"
DEVICE="enp0s9"
ONBOOT="yes"

IPADDR="192.168.57.4"
MASK="255.255.255.0"
```
```
[florian@localhost network-scripts]$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:52:03:ef brd ff:ff:ff:ff:ff:ff
    inet 10.0.2.15/24 brd 10.0.2.255 scope global dynamic noprefixroute enp0s3
       valid_lft 84466sec preferred_lft 84466sec
    inet6 fe80::fd43:f340:e369:ff69/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:c9:40:c1 brd ff:ff:ff:ff:ff:ff
    inet 192.168.57.3/24 brd 192.168.57.255 scope global noprefixroute enp0s8
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fec9:40c1/64 scope link 
       valid_lft forever preferred_lft forever
4: enp0s9: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:69:a1:fd brd ff:ff:ff:ff:ff:ff
    inet 192.168.57.4/24 brd 192.168.57.255 scope global noprefixroute enp0s9
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fe69:a1fd/64 scope link 
       valid_lft forever preferred_lft forever
```
```
[florian@localhost network-scripts]$ ip route
default via 10.0.2.2 dev enp0s3 proto dhcp metric 100 
10.0.2.0/24 dev enp0s3 proto kernel scope link src 10.0.2.15 metric 100 
192.168.57.0/24 dev enp0s8 proto kernel scope link src 192.168.57.3 metric 101 
192.168.57.0/24 dev enp0s9 proto kernel scope link src 192.168.57.4 metric 102
```
```
[florian@localhost network-scripts]$ sudo ip n s
10.0.2.3 dev enp0s3 lladdr 52:54:00:12:35:03 STALE
10.0.2.2 dev enp0s3 lladdr 52:54:00:12:35:02 REACHABLE
192.168.57.1 dev enp0s9 lladdr 0a:00:27:00:00:02 REACHABLE
192.168.57.1 dev enp0s8 lladdr 0a:00:27:00:00:01 REACHABLE
```

🐙 mettre en place un NIC teaming (ou bonding):

Jé pa réusi :cry: 

### 2. Serveur SSH

Modification la configuration du système pour que le serveur SSH tourne sur le port 2222:

```
[florian@localhost ~]$ ss -tnlp
State      Recv-Q      Send-Q            Local Address:Port           Peer Address:Port      
LISTEN     0           128                     0.0.0.0:2222                0.0.0.0:*         
LISTEN     0           128                        [::]:2222                   [::]:* 
```

```
[florian@localhost ~]$ sudo !!
sudo firewall-cmd --list-all
[sudo] password for florian: 
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: bond0 enp0s3 enp0s8 enp0s9
  sources: 
  services: cockpit dhcpv6-client ssh
  ports: 2222/tcp
  protocols: 
  masquerade: no
  forward-ports: 
  source-ports: 
  icmp-blocks: 
  rich rules: 
```


Analyse des trames de connexion au serveur SSH:
(j'ai cassé mon VirtualBox en installant Vagrant (qui ne marche pas non plus)... :cry: :scream:)

```
sudo tcpdump -i enp0s8
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on enp0s8, link-type EN10MB (Ethernet), capture size 262144 bytes
16:37:03.103407 ARP, Request who-has localhost.localdomain tell 192.168.56.104, length 46 
16:37:03.103421 ARP, Reply localhost.localdomain is-at 08:00:27:6f:54:c9 (oui Unknown), length 28
16:37:03.103612 IP 192.168.56.104.50800 > localhost.localdomain.ssh: Flags [S], seq 3590438451, win 29200, options [mss 1460,sackOK,TS val 2376396728 ecr 0,nop,wscale 7], length 0
16:37:03.103643 IP localhost.localdomain.ssh > 192.168.56.104.50800: Flags [R.], seq 0, ack 3590438452, win 0, length 0
16:37:08.550845 ARP, Request who-has 192.168.56.104 tell localhost.localdomain, length 28
16:37:08.551000 ARP, Reply 192.168.56.104 is-at 08:00:27:a3:c2:75 (oui Unknown), length 46
```
On voit quatre ligne ARP. 
La première et la seconde ce sont des demandes.
La deuxième et la quatrième ce sont des réponses.

## III. Routage Simple

                   +-------+
                   |Outside|
                   | world |
                   +---+---+
                       |
                       |
+-------+         +----+---+         +-------+
|       |   net1  |        |   net2  |       |
|  VM1  +---------+ Router +---------+  VM2  |
|       |         |        |         |       |
+-------+         +--------+         +-------+


(Mon virtualBox toujours Cassé on va faire sans !)

### Conf:

VM1: Carte1 = ip 192.168.1.1 | mask 255.255.255.252
VM2: Carte1 = ip 192.168.2.1 | mask 255.255.255.252

Routeur: Carte1 = ip 192.168.1.2 | mask 255.255.255.252 | Co VM1
         Carte2 = ip 192.168.2.2 | mask 255.255.255.252 | Co VM2
         Carte3 = ip 192.168.3.2 | mask 255.255.255.0   | Co Host pour internet
         
Ensuite on add les routes:

Fichier à modif:
``` vim /etc/sysconfig/network-scripts/route-eth0 ```

pour VM1:
``` 192.168.3.0/24 via 192.168.1.2 dev eth0 ```

pour VM2:
``` 192.168.3.0/24 via 192.168.2.2 dev eth0 ```

### Traceroute:

Commande:

```
traceroute 8.8.8.8
```

Theoriquement la on à quelque chose comme ça:
(mais avec nos IP au début, la notre, celle du routeur, celle de mon host)

```
traceroute to google.com (74.125.236.132), 30 hops max, 60 byte packets
1  220.224.141.129 (220.224.141.129)  89.174 ms  89.094 ms  89.054 ms
2  115.255.239.65 (115.255.239.65)  109.037 ms  108.994 ms  108.963 ms
3  124.124.251.245 (124.124.251.245)  108.937 ms  121.322 ms  121.300 ms
4  * 115.255.239.45 (115.255.239.45)  113.754 ms  113.692 ms
5  72.14.212.118 (72.14.212.118)  123.585 ms  123.558 ms  123.527 ms
6  72.14.232.202 (72.14.232.202)  123.499 ms  123.475 ms  143.523 ms
7  216.239.48.179 (216.239.48.179)  143.503 ms  95.106 ms  95.026 ms
8  bom03s02-in-f4.1e100.net (74.125.236.132)  94.980 ms  104.989 ms  104.954 m
```

## Métrologie:

(vm vroken :-1:)

Bon du coup j'ai matter un peu Cockpit sur la toile.

C'est un service web qui permet de faire du monitoring sur ou des serveurs.
On peut voir l'espace du disque, l'utilisation du réseau, la consomation CPU de chaque services ect ...

De base Cockpit écoute sur le port '9090'.
