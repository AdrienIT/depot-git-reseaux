# TP 5 - Une "vraie" topologie ?

# I. Toplogie 1 - intro VLAN

## 1. Mise en place de la topologie

Done !

## 2. Setup clients

```
guest1> ping 10.5.20.12
84 bytes from 10.5.20.12 icmp_seq=1 ttl=64 time=0.416 ms
84 bytes from 10.5.20.12 icmp_seq=2 ttl=64 time=0.659 ms
84 bytes from 10.5.20.12 icmp_seq=3 ttl=64 time=0.676 ms
84 bytes from 10.5.20.12 icmp_seq=4 ttl=64 time=0.606 ms
^C
guest1>
```

```
admin1> ping 10.5.10.12
84 bytes from 10.5.10.12 icmp_seq=1 ttl=64 time=0.409 ms
84 bytes from 10.5.10.12 icmp_seq=2 ttl=64 time=0.641 ms
84 bytes from 10.5.10.12 icmp_seq=3 ttl=64 time=0.554 ms
84 bytes from 10.5.10.12 icmp_seq=4 ttl=64 time=0.617 ms
^C
admin1>
```

→ Tout le monde se ping c'est génial.

## 3. Setup VLANs

![](https://i.imgur.com/w77jjc0.png)

Et pour les trunk : 

```
IOU1#show int trunk

Port        Mode             Encapsulation  Status        Native vlan
Et0/2       on               802.1q         trunking      1

Port        Vlans allowed on trunk
Et0/2       1-4094

Port        Vlans allowed and active in management domain
Et0/2       1,10,20

Port        Vlans in spanning tree forwarding state and not pruned
Et0/2       none
IOU1#
```

C'est pareil de l'autre coté.

Les guest peuvent se joindre, idem pour les admins et si le guest essaye de passer dans les réseau admin, cela ne morch po

```
guest1> ping 10.5.20.12
84 bytes from 10.5.20.12 icmp_seq=1 ttl=64 time=0.491 ms
84 bytes from 10.5.20.12 icmp_seq=2 ttl=64 time=0.599 ms
84 bytes from 10.5.20.12 icmp_seq=3 ttl=64 time=0.560 ms
^C
guest1> ip 10.5.10.13 255.255.255.0
Checking for duplicate address...
PC1 : 10.5.10.13 255.255.255.0

guest1> ping 10.5.10.11
host (10.5.10.11) not reachable

guest1>
```

# II. Topologie 2 - VLAN, sous-interface, NAT

## 2. Adressage

Guest1 → Guest2 → Guest3

```
guest1> ping 10.5.20.12
84 bytes from 10.5.20.12 icmp_seq=1 ttl=64 time=0.450 ms
84 bytes from 10.5.20.12 icmp_seq=2 ttl=64 time=0.571 ms
^C
guest1> ping 10.5.20.13
84 bytes from 10.5.20.13 icmp_seq=1 ttl=64 time=0.738 ms
84 bytes from 10.5.20.13 icmp_seq=2 ttl=64 time=0.876 ms
^C
guest1>

```

Admin1 → Admin2 → Admin3

```
admin1> ping 10.5.10.12
84 bytes from 10.5.10.12 icmp_seq=1 ttl=64 time=0.430 ms
^C
admin1> ping 10.5.10.13
84 bytes from 10.5.10.13 icmp_seq=1 ttl=64 time=0.611 ms
84 bytes from 10.5.10.13 icmp_seq=2 ttl=64 time=1.182 ms
^C
admin1>
```

## 3. VLAN

Les vlans sont up sur les sw1 et sw2, on le fait sur le sw3 et tout marche.

![](https://i.imgur.com/bjNY5Df.png)



Le guest qui change de network ne peut pas ping les admins.

```
guest1> ip 10.5.10.15 255.255.255.0
Checking for duplicate address...
PC1 : 10.5.10.15 255.255.255.0

guest1> ping 10.5.10.11
^Chost (10.5.10.11) not reachable

guest1>
```

## 4. Sous-interfaces

Sous-interfaces conigurés : 

```
R1#show ip int br
Interface                  IP-Address      OK? Method Status                Protocol
FastEthernet0/0            unassigned      YES unset  administratively down down
FastEthernet0/0.10         unassigned      YES unset  administratively down down
FastEthernet1/0            unassigned      YES unset  up                    up
FastEthernet1/0.10         10.5.10.254     YES manual up                    up
FastEthernet1/0.20         10.5.20.254     YES manual up                    up
```

Les guests ping leur passerelle : 

```
guest1> ping 10.5.20.254
84 bytes from 10.5.20.254 icmp_seq=1 ttl=255 time=8.932 ms
84 bytes from 10.5.20.254 icmp_seq=2 ttl=255 time=6.993 ms
^C
guest1>
```

idem pour les admins : 

```
admin1> ping 10.5.10.254
84 bytes from 10.5.10.254 icmp_seq=1 ttl=255 time=9.464 ms
84 bytes from 10.5.10.254 icmp_seq=2 ttl=255 time=8.548 ms
^C
admin1>

```

## 5. NAT

Config du nat réussie : 

```
R1#show ip int br
Interface                  IP-Address      OK? Method Status                Protocol
FastEthernet0/0            unassigned      YES unset  administratively down down
FastEthernet0/0.10         unassigned      YES unset  administratively down down
FastEthernet1/0            unassigned      YES unset  up                    up
FastEthernet1/0.10         10.5.10.254     YES manual up                    up
FastEthernet1/0.20         10.5.20.254     YES manual up                    up
NVI0
```

Admin peut ping la toile : 

```
admin1> ping 8.8.8.8
*10.5.10.254 icmp_seq=1 ttl=255 time=7.527 ms (ICMP type:3, code:1, Destination host unreachable)
*10.5.10.254 icmp_seq=2 ttl=255 time=7.109 ms (ICMP type:3, code:1, Destination host unreachable)
^C
admin1>
```

Idem pour les clients : 

```
guest1> ping 8.8.8.8
*10.5.20.254 icmp_seq=1 ttl=255 time=9.601 ms (ICMP type:3, code:1, Destination host unreachable)
*10.5.20.254 icmp_seq=2 ttl=255 time=7.591 ms (ICMP type:3, code:1, Destination host unreachable)
^C
guest1>

```

# III. Topologie 3 - Ajouter des services

## 1. Mise en place de la topologie

![](https://i.imgur.com/fezUkJg.png)

## 2. Basic setup

 - Guest ping DNS

```
guest1> ping 10.5.30.11
84 bytes from 10.5.30.11 icmp_seq=1 ttl=63 time=15.389 ms
84 bytes from 10.5.30.11 icmp_seq=2 ttl=63 time=12.529 ms
84 bytes from 10.5.30.11 icmp_seq=3 ttl=63 time=13.641 ms
84 bytes from 10.5.30.11 icmp_seq=4 ttl=63 time=19.059 ms
^C
guest1>

```

 - Guest ping Server web

```
guest1> ping 10.5.30.12
84 bytes from 10.5.30.12 icmp_seq=1 ttl=63 time=30.412 ms
84 bytes from 10.5.30.12 icmp_seq=2 ttl=63 time=12.279 ms
84 bytes from 10.5.30.12 icmp_seq=3 ttl=63 time=18.363 ms
84 bytes from 10.5.30.12 icmp_seq=4 ttl=63 time=17.891 ms
^C
guest1>
```

 - Admin ping DNS

```
admin1> ping 10.5.30.11
84 bytes from 10.5.30.11 icmp_seq=1 ttl=63 time=18.916 ms
84 bytes from 10.5.30.11 icmp_seq=2 ttl=63 time=15.760 ms
84 bytes from 10.5.30.11 icmp_seq=3 ttl=63 time=19.635 ms
84 bytes from 10.5.30.11 icmp_seq=4 ttl=63 time=20.841 ms
^C
admin1>
```

 - Admin ping Server web

```
admin1> ping 10.5.30.12
84 bytes from 10.5.30.12 icmp_seq=1 ttl=63 time=18.042 ms
84 bytes from 10.5.30.12 icmp_seq=2 ttl=63 time=14.513 ms
84 bytes from 10.5.30.12 icmp_seq=3 ttl=63 time=17.803 ms
84 bytes from 10.5.30.12 icmp_seq=4 ttl=63 time=14.846 ms
^C
admin1>
```

## 3. Serveur DHCP

Guest 3 prend une ip via le serveur dhcp : 

```
guest3> ip dhcp
DDORA IP 10.5.20.100/24 GW 10.5.20.254

guest3>
```

## 4. Serveur Web

Curl localhost:80

![](https://i.imgur.com/1Le9vT4.png)

Pour curl depuis on autre machine, j'ai ajouté une VM à mon switch 1, dans mon vlan 20. Je curl depuis ma VM et boom : 

![](https://i.imgur.com/l0jFcs1.png)

## 5. Serveur DNS

Afin de mettre en place le serveur dns, il faut installer le bind et bind-utils package, une fois fais, on edit le fichier de conf`/var/named`. Il suffit ensuite de créer un fichier `tp5.b1.db` et c'est ici qu'on ajoute les noms de domaines et les ip respectives


# Pour aller plus loin...

## DHCP snooping

le DHCP snooping permet de definir un serveur DHCP précis. Les serveurs DHCp "pirates" n'auront donc aucun moyen de se faire passer pour le serveur dhcp et de voler des données. Cela se passe dans les requests OFFER.

Pour le config : 
conf t
ip dhcp dooping

## IP Source Guard

L'IP Source Guard est lié au DHCP snooping car il agit sur les ip sources configurés et le DHCP snooping binding database. Il s'occuppe de lié l'addresse mac d'une machine à son adresse IP. Grâce à ça, plus aucun moyen pour l'attaquant de se faire passer pour un serveur DHCP.

Pour la config:
conf t
ip source binding (AMC) vlan (vlan) (ip) int (int)
ip verify source dans la conf lié à l'interface

## VLAN Access Map

Le VLAN Access Map nous permet de filtrer le trafic entrant et sortant dans un switch Vlan. On peut l'appliquer à la 3 eme couche mais aussi à une MAC-addresse liste

Il faut configurer une liste (1) avec des IPS qui peuvent communiquer entre elles, et une autre qui peut aussi communiquer entre elles (admin et guest dans notre cas) mais on ne veut pas que admin et guest communique. 


---


# Fin du tp5