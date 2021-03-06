# Tp1 : Adrien Zoghbi

## I. Exploration locale en solo

### 1. Affichage d'informations sur la pile TCP/IP locale

#### **Affichez les infos des cartes réseau de votre PC : **

(En ligne de commandes)

-> Commandes
```
ip a
```

-> Résulats
```
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: wlan0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
    link/ether 04:ea:56:21:65:4c brd ff:ff:ff:ff:ff:ff
    inet 10.33.2.43/22 brd 10.33.3.255 scope global dynamic noprefixroute wlan0
       valid_lft 3571sec preferred_lft 3571sec
    inet6 fe80::cedf:213:5c4b:296e/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
```
nom: wlan0
Adresse MAC : 04:ea:56:21:65:4c
Adresse ip : 10.33.2.43

> Pas de carte ethernet

#### **Affichez votre gateway : **

-> Commandes
```
netstat -nr
```
-> Résultat
```
Table de routage IP du noyau
Destination     Passerelle      Genmask         Indic   MSS Fenêtre irtt Iface
0.0.0.0         10.33.3.253     0.0.0.0         UG        0 0          0 wlan0
10.33.0.0       0.0.0.0         255.255.252.0   U         0 0          0 wlan0
```
Ip de la passerelle lié à la carte wifi : 10.33.3.253

(Graphicalement)

Configuration d'ordinateur : 
Linux parrot 5.4.0-1parrot1-amd64 #1 SMP Parrot 5.4.6-1parrot1 (2019-12-30) x86_64 GNU/Linux (via la commande uname -a)

Screenshot : https://imgur.com/a/Uuq95OS

> Système>Préférences>Internet et réseau>Configuration réseau avancée.
> Selection du wifi> Parametre
> Ps : Parrot OS ne permet pas de voir l'adresse ip d'une carte wifi graphicalement.


Question :  à quoi sert la gateway dans le réseau d'YNOV ?

Dans le rseau d'YNOV, la gateway (passerelle par défaut) sert à faire la liaison entre 2 réseaux.

### 2. Modifications des informations

#### **A. Modification d'adresse IP (part 1)**

> L'utilisation de Parrot OS ne permet pas de changer l'adresse ip graphiquement.
> On pourrait le faire en en passant par la modification de ce fichier : 
etc/network/interfaces


**Explication de la perte de connexion :**


En changeant d'adresse ip, il se peut que l'on change de masque de sous-réseaux, exemple :
Passer de 255.255.255.0 à 255.255.0.0
Cela serait long à expliquer et ce n'est pas le sujet, mais les masques de sous-réseaux sont découper en fonction du dernier octet.
Si notre masque de sous-réseaux ne change pas, on ne pert pas la conection inter,si on change, on perd la connection.

#### **B.NMAP**

Utilisez nmap pour scanner le réseau de votre carte WiFi et trouver une adresse IP libre :

-> Commandes
```
nmap -sP 10.33.0.0/22
```
-> Résultas
```
https://pastebin.com/mZXXJW34
```

#### C. Modification d'adresse IP (part 2)

Après le scan nmap, on voit plusieurs adresses ip disponible.
Je suis passé de :
10.33.2.43  à 10.33.0.15

Nouveau scan snap :
-> Commandes
```
nmap -sP 10.33.0.0/22
```
-> Résultas
```
Le résultat est le meme qu'avant.
```

## II. Exploration locale en duo

A faire plus tard

## III. Manipulations d'autres outils/protocoles côté client

### 1. DHCP

-> Afficher l'adresse IP du serveur DHCP du réseau WiFi YNOV

Commandes
```
sudo dhclient -v wlan0
```
Résultats
```
Internet Systems Consortium DHCP Client 4.4.1
Copyright 2004-2018 Internet Systems Consortium.
All rights reserved.
For info, please visit https://www.isc.org/software/dhcp/

Listening on LPF/wlan0/04:ea:56:21:65:4c
Sending on   LPF/wlan0/04:ea:56:21:65:4c
Sending on   Socket/fallback
DHCPREQUEST for 10.33.2.86 on wlan0 to 255.255.255.255 port 67
DHCPACK of 10.33.2.86 from 10.33.3.254
RTNETLINK answers: File exists
bound to 10.33.2.86 -- renewal in 1360 seconds.
```
La ligne qui nous intéresse :
```
DHCPACK of 10.3cat dhclient-f6160ce6-9132-4c76-b7b2-9c6e9cbb88f1-wlan0.lease3.2.86 from 10.33.3.254
```
Cette commande nous permet de demander une nouvelle adresse ip au serveur dhcp.
Ici, on remarque qu'il nous l'adresse ip : "10.33.2.86" depuisle serveur dhcp : "10.33.3.254"

-> BAIL DHCP
Pour exemple, avec NetworkManager, on peut faire cette commande :
```
cat dhclient-f6160ce6-9132-4c76-b7b2-9c6e9cbb88f1-wlan0.lease
```
Ce qui nous donne
```
lease {
  interface "wlan0";
  fixed-address 192.168.20.116;
  option subnet-mask 255.255.255.0;
  option wpad 22:a:22;
  option dhcp-lease-time 86400;
  option routers 192.168.20.1;
  option dhcp-message-type 5;
  option dhcp-server-identifier 192.168.20.1;
  option domain-name-servers 192.168.20.1;
  option dhcp-renewal-time 43200;
  option dhcp-rebinding-time 75600;
  option broadcast-address 192.168.20.255;
  option host-name "parrot";
  renew 4 2019/11/14 05:55:29;
  rebind 4 2019/11/14 17:19:08;
  expire 4 2019/11/14 20:19:08;
}
lease {
  interface "wlan0";
  fixed-address 192.168.20.116;
  option wpad 22:a:22;
  option subnet-mask 255.255.255.0;
  option routers 192.168.20.1;
  option dhcp-lease-time 86400;
  option dhcp-message-type 5;
  option domain-name-servers 192.168.20.1;
  option dhcp-server-identifier 192.168.20.1;
  option dhcp-renewal-time 43200;
  option broadcast-address 192.168.20.255;
  option dhcp-rebinding-time 75600;
  option host-name "parrot";
  renew 4 2019/11/14 08:38:29;
  rebind 4 2019/11/14 18:09:41;
  expire 4 2019/11/14 21:09:41;

```

La ligne qui nous intéresse: 
```
renew 4 2019/11/14 08:38:29;
rebind 4 2019/11/14 18:09:41;
expire 4 2019/11/14 21:09:41;
```

-> Demandez une nouvelle adresse IP
Commandes
```
sudo dhclient -v wlan0
```
Le résultat est un peu plus haut.
 --> DHCPACK of 10.33.2.86 from 10.33.3.254

### 2. DNS

-> Trouver l'ip serveur dns que notre ordi connait : 

Commande
```
ip r | grep default
```
Résultat
```
default via 10.33.3.253 dev wlan0
```
Notre ordinateur connait 10.33.3.253.

#### lookup

-> lookup google.com
Commande
```
dig google.com
```
Résultat
```
; <<>> DiG 9.11.5-P4-5.1+b1-Debian <<>> google.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 51547
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;google.com.			IN	A

;; ANSWER SECTION:
google.com.		245	IN	A	216.58.201.238

;; Query time: 4 msec
;; SERVER: 10.33.10.148#53(10.33.10.148)
;; WHEN: mer. janv. 22 10:05:44 CET 2020
;; MSG SIZE  rcvd: 55
```

Dns google : 216.58.201.238

->Lookup ynov.com
Commande
```
dig ynov.com
```
Résultat
```
; <<>> DiG 9.11.5-P4-5.1+b1-Debian <<>> ynov.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 58069
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;ynov.com.			IN	A

;; ANSWER SECTION:
ynov.com.		2329	IN	A	217.70.184.38

;; Query time: 5 msec
;; SERVER: 10.33.10.148#53(10.33.10.148)
;; WHEN: mer. janv. 22 10:15:34 CET 2020
;; MSG SIZE  rcvd: 53

```

Dns Ynov : 217.70.184.38

On remarque que les résultats sont différents car notre "odinateur" ne passe pas par le meme chemin pour aller sur google.com et sur Ynov.com

#### Reverse LOOKUP

-> reverse lookup 78.74.21.21
Commande
```
dig -x 78.74.21.21
```
Résultat
```
; <<>> DiG 9.11.5-P4-5.1+b1-Debian <<>> -x 78.74.21.21
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 45166
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;21.21.74.78.in-addr.arpa.	IN	PTR

;; ANSWER SECTION:
21.21.74.78.in-addr.arpa. 3490	IN	PTR	host-78-74-21-21.homerun.telia.com.

;; Query time: 5 msec
;; SERVER: 10.33.10.148#53(10.33.10.148)
;; WHEN: mer. janv. 22 10:20:48 CET 2020
;; MSG SIZE  rcvd: 101
```

Serveur : host-78-74-21-21.homerun.telia.com.

-> reverse Lookup 92.146.54.88
Commande
```
dig -x 92.146.54.88
```
Résultat
```
; <<>> DiG 9.11.5-P4-5.1+b1-Debian <<>> -x 92.146.54.88
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 19911
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;88.54.146.92.in-addr.arpa.	IN	PTR

;; ANSWER SECTION:
88.54.146.92.in-addr.arpa. 3425	IN	PTR	apoitiers654-1-167-88.w92-146.abo.wanadoo.fr.

;; Query time: 6 msec
;; SERVER: 10.33.10.148#53(10.33.10.148)
;; WHEN: mer. janv. 22 10:22:12 CET 2020
;; MSG SIZE  rcvd: 113
```

Serveur : apoitiers654-1-167-88.w92-146.abo.wanadoo.fr.

Grace  à la commande dig-x (qui permet de faire un reverse lookup), on peutobtenir facilement le serveur derriere une adresse ip.