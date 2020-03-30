# TP4 : Adrien ZOGHBI

# TP 4 - Cisco, Routage, DHCP

## I. Topologie 1 : simple

### 2. Mise en place

#### A. Topologie GNS3

![](https://i.imgur.com/y5QoeIy.png)

#### B. Définition d'IPs statiques

- Vérification

🌞 Vérifier et PROUVER que :

- guest1 peut joindre le routeur

```
guest1> ping 10.4.2.254
84 bytes from 10.4.2.254 icmp_seq=1 ttl=255 time=10.345 ms
84 bytes from 10.4.2.254 icmp_seq=2 ttl=255 time=5.800 ms
84 bytes from 10.4.2.254 icmp_seq=3 ttl=255 time=4.001 ms
84 bytes from 10.4.2.254 icmp_seq=4 ttl=255 time=2.296 ms
^C
guest1>
```


- admin1 peut joindre le routeur
     
![](https://i.imgur.com/DfMQMKa.png)

- router1 peut joindre les deux autres machines
     
```
R1#ping 10.4.2.11

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.4.2.11, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 32/32/36 ms
R1#ping 10.4.1.11

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.4.1.11, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 36/40/44 ms
R1#

```

- vérifier la table ARP de router1

```
R1#show arp
Protocol  Address          Age (min)  Hardware Addr   Type   Interface
Internet  10.4.1.11               0   0800.2727.ef91  ARPA   FastEthernet1/0
Internet  10.4.2.11               5   0050.7966.6800  ARPA   FastEthernet0/0
Internet  10.4.1.254              -   cc01.0481.0010  ARPA   FastEthernet1/0
Internet  10.4.2.254              -   cc01.0481.0000  ARPA   FastEthernet0/0
R1#
```

#### C. Routage

🌞 Ajouter une route sur admin1 pour qu'il puisse joindre le réseau guests

![](https://i.imgur.com/uyhbehn.png)

🌞 Ajouter une route sur guest1 pour qu'il puisse joindre le réseau admin

```
guest1> ping 10.4.1.11
84 bytes from 10.4.1.11 icmp_seq=1 ttl=63 time=17.074 ms
84 bytes from 10.4.1.11 icmp_seq=2 ttl=63 time=12.782 ms
^C
guest1>
```

## II. Topologie 2 : dumb switches

### 1. Présentation de la topo

#### 2. Mise en place

##### C. Vérification

```
guest1> ping 10.4.1.11
84 bytes from 10.4.1.11 icmp_seq=1 ttl=63 time=15.136 ms
84 bytes from 10.4.1.11 icmp_seq=2 ttl=63 time=19.254 ms
84 bytes from 10.4.1.11 icmp_seq=3 ttl=63 time=19.694 ms
^C
guest1>
```


TOPOLOGIE

![](https://i.imgur.com/40NUs2i.png)

## III. Topologie 3 : adding nodes and NAT

### 1. Présentation de la topo

#### B. VPCS

#### "🌞 Configurer les VPCS" 

Définition addresse statique

→ Guest2 : ip 10.4.2.12/24 10.4.2.254 + save

→ Guest3 : ip 10.4.2.13/24 10.4.2.254 + save


- Ajout guest 2

```
guest2> ping 10.4.1.11
84 bytes from 10.4.1.11 icmp_seq=1 ttl=63 time=19.982 ms
84 bytes from 10.4.1.11 icmp_seq=2 ttl=63 time=20.729 ms
84 bytes from 10.4.1.11 icmp_seq=3 ttl=63 time=17.022 ms
84 bytes from 10.4.1.11 icmp_seq=4 ttl=63 time=14.839 ms
84 bytes from 10.4.1.11 icmp_seq=5 ttl=63 time=17.218 ms

guest2>
```

- Ajout guest 3

```
guest3> ping 10.4.1.11
10.4.1.11 icmp_seq=1 timeout
84 bytes from 10.4.1.11 icmp_seq=2 ttl=63 time=15.801 ms
84 bytes from 10.4.1.11 icmp_seq=3 ttl=63 time=14.810 ms
84 bytes from 10.4.1.11 icmp_seq=4 ttl=63 time=16.165 ms
84 bytes from 10.4.1.11 icmp_seq=5 ttl=63 time=14.775 ms

guest3>
```

#### C. Accès WAN

"🌞 Configurer le routeur"

 - récupérer une IP en DHCP sur l'interface branchée au nuage NAT

```
conf t
interface fastethernet 2/0
ip address dhcp
```

```
R1#show ip int br
Interface                  IP-Address      OK? Method Status                Protocol
FastEthernet0/0            10.4.1.254      YES NVRAM  up                    up
FastEthernet1/0            10.4.2.254      YES NVRAM  up                    up
FastEthernet2/0            192.168.122.184 YES DHCP   up                    up
NVI0
```


Accès NAT réussi :




🌞 Vérifier et PROUVER que :

→ le routeur a un accès WAN (internet)

```
R1#ping 8.8.8.8

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 8.8.8.8, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 64/67/72 ms
R1#
```

→ tous les clients du réseau admins (y'en a toujours qu'un) et ceux du réseau guests ont un accès WAN (internet)

![](https://i.imgur.com/zenthpt.png)


→ tous les clients du réseau ont de la résolution de noms grâce au serveur DNS configuré

 - Guest 1
```
guest1> ping google.fr
google.fr resolved to 172.217.18.195
84 bytes from 172.217.18.195 icmp_seq=1 ttl=54 time=30.613 ms
84 bytes from 172.217.18.195 icmp_seq=2 ttl=54 time=31.889 ms
84 bytes from 172.217.18.195 icmp_seq=3 ttl=54 time=27.483 ms
84 bytes from 172.217.18.195 icmp_seq=4 ttl=54 time=26.526 ms
^C
guest1>
```

 - Guest 2

```
guest2> ping google.fr
google.fr resolved to 172.217.18.195
84 bytes from 172.217.18.195 icmp_seq=1 ttl=54 time=29.851 ms
84 bytes from 172.217.18.195 icmp_seq=2 ttl=54 time=24.773 ms
84 bytes from 172.217.18.195 icmp_seq=3 ttl=54 time=29.918 ms
84 bytes from 172.217.18.195 icmp_seq=4 ttl=54 time=26.500 ms
^C
guest2>
```

 - Guest 3

```
guest3> ping google.fr
google.fr resolved to 172.217.18.195
84 bytes from 172.217.18.195 icmp_seq=1 ttl=54 time=31.705 ms
84 bytes from 172.217.18.195 icmp_seq=2 ttl=54 time=28.843 ms
84 bytes from 172.217.18.195 icmp_seq=3 ttl=54 time=23.639 ms
84 bytes from 172.217.18.195 icmp_seq=4 ttl=54 time=21.626 ms
^C
guest3>
```

## IV. Topologie 4 : home-made DHCP

Port 67 (port 67 car c'est le port qui permet d'écouter en étant serveur)

![](https://i.imgur.com/bjxra7N.png)

Attribution d'addresse IP DHCP

```
guest2> ip dhcp
DDORA IP 10.4.2.101/24 GW 10.4.2.254
```

Preuve de la capture et du DORA

![](https://i.imgur.com/H7GLoqq.png)

# FIN DU TP 4