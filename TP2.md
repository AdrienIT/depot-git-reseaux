# TP 3 Adrien ZOGHBI

## TP 3 - Routage, ARP, Spéléologie réseau

### Préparation de l'environnement

- 🌞 Prouvez que chacun des points de la préparation de l'environnement ci-dessus ont été respectés :  




#### Carte NAT désactivée : 
 - Client : ![](https://i.imgur.com/dZZ02aq.png)

 - Serveur : ![](https://i.imgur.com/aYA9ZrO.png)

 - Routeur : ![](https://i.imgur.com/obY6VNU.png)



#### Serveur SSH fonctionnel qui écoute sur le port 7777/tcp

 - ![Uploading file..._tqvbaa6xc]()


#### Pare-feu activé et configuré

 - ![](https://i.imgur.com/nweXH74.png)


#### Nom configuré	
 - Client
```
[root@client1 ~]# hostname
client1.net1.tp3

```
 - Server
```
[root@server1 ~]# hostname
server1.net2.tp3
```
 - Router
```
[root@router ~]# hostname
router.tp3
```

#### Fichiers /etc/hosts de toutes les machines configurés 	

- Client

```
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6

10.3.1.11 client1.net1.tp3

```
- Serveur

```
[root@server1 ~]# cat /etc/hosts
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6

10.3.2.11 server1.net2.tp3

```

- Router

```
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.loca$
::1         localhost localhost.localdomain localhost6 localhost6.loca$

10.3.1.254 router.tp3

10.3.2.254 router.tp3

```

#### Réseaux et adressage des machines

 -> client1 <> router

 - - depuis client1, ping router doit marcher

```
[root@client1 ~]# ping router
PING router (10.3.1.254) 56(84) bytes of data.
64 bytes from router (10.3.1.254): icmp_seq=1 ttl=64 time=0.390 ms
64 bytes from router (10.3.1.254): icmp_seq=2 ttl=64 time=0.326 ms
64 bytes from router (10.3.1.254): icmp_seq=3 ttl=64 time=0.338 ms
64 bytes from router (10.3.1.254): icmp_seq=4 ttl=64 time=0.957 ms
^C
--- router ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3013ms
rtt min/avg/max/mdev = 0.326/0.502/0.957/0.264 ms
[root@client1 ~]#
```

 - - depuis router, ping client1 doit marcher

```
[root@router ~]# ping client1
PING client1 (10.3.1.11) 56(84) bytes of data.
64 bytes from client1 (10.3.1.11): icmp_seq=1 ttl=64 time=0.441 ms
64 bytes from client1 (10.3.1.11): icmp_seq=2 ttl=64 time=0.940 ms
64 bytes from client1 (10.3.1.11): icmp_seq=3 ttl=64 time=0.838 ms
64 bytes from client1 (10.3.1.11): icmp_seq=4 ttl=64 time=0.514 ms
^C
--- client1 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3010ms
rtt min/avg/max/mdev = 0.441/0.683/0.940/0.211 ms
[root@router ~]#
```

 -> server1 <> router

 - - depuis server1, ping router doit marcher
 
```
[root@server1 ~]# ping router
PING router (10.3.2.254) 56(84) bytes of data.
64 bytes from router (10.3.2.254): icmp_seq=1 ttl=64 time=0.387 ms
64 bytes from router (10.3.2.254): icmp_seq=2 ttl=64 time=0.379 ms
64 bytes from router (10.3.2.254): icmp_seq=3 ttl=64 time=1.27 ms
64 bytes from router (10.3.2.254): icmp_seq=4 ttl=64 time=0.553 ms
^C
--- router ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3018ms
rtt min/avg/max/mdev = 0.379/0.648/1.275/0.369 ms
[root@server1 ~]#
```
 
 - - depuis router, ping server1 doit marcher

```
[root@router ~]# ping server1
PING server1 (10.3.2.11) 56(84) bytes of data.
64 bytes from server1 (10.3.2.11): icmp_seq=1 ttl=64 time=0.500 ms
64 bytes from server1 (10.3.2.11): icmp_seq=2 ttl=64 time=0.387 ms
64 bytes from server1 (10.3.2.11): icmp_seq=3 ttl=64 time=0.966 ms
64 bytes from server1 (10.3.2.11): icmp_seq=4 ttl=64 time=0.823 ms
^C
--- server1 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3008ms
rtt min/avg/max/mdev = 0.387/0.669/0.966/0.234 ms
[root@router ~]#

```

### I. Mise en place du routage

#### 1. Configuration du routage sur router

🌞 Effectuez cette commande sur la machine router.

```
[root@router ~]# sudo sysctl -w net.ipv4.conf.all.forwarding=1
net.ipv4.conf.all.forwarding = 1
[root@router ~]#
```

#### 2. Ajouter les routes statiques

```
[root@client1 ~]# ip r s
10.3.1.0/24 dev enp0s8 proto kernel scope link src 10.3.1.11 metric 101
10.3.2.0/24 via 10.3.1.254 dev enp0s8 proto static metric 101
[root@client1 ~]# ping 10.3.2.11
PING 10.3.2.11 (10.3.2.11) 56(84) bytes of data.
64 bytes from 10.3.2.11: icmp_seq=1 ttl=63 time=0.874 ms
64 bytes from 10.3.2.11: icmp_seq=2 ttl=63 time=0.751 ms
64 bytes from 10.3.2.11: icmp_seq=3 ttl=63 time=1.16 ms
64 bytes from 10.3.2.11: icmp_seq=4 ttl=63 time=0.793 ms
^C
--- 10.3.2.11 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3004ms
rtt min/avg/max/mdev = 0.751/0.894/1.160/0.162 ms
[root@client1 ~]# traceroute 10.3.2.11
traceroute to 10.3.2.11 (10.3.2.11), 30 hops max, 60 byte packets
 1  router (10.3.1.254)  0.320 ms  0.258 ms  0.223 ms
 2  router (10.3.1.254)  0.140 ms !X  0.232 ms !X  0.229 ms !X
[root@client1 ~]#
```

```
[root@server1 ~]# ip r s
10.3.1.0/24 via 10.3.2.254 dev enp0s8 proto static metric 101
10.3.2.0/24 dev enp0s8 proto kernel scope link src 10.3.2.11 metric 101
[root@server1 ~]# ping 10.3.1.11
PING 10.3.1.11 (10.3.1.11) 56(84) bytes of data.
64 bytes from 10.3.1.11: icmp_seq=1 ttl=63 time=0.838 ms
64 bytes from 10.3.1.11: icmp_seq=2 ttl=63 time=1.22 ms
64 bytes from 10.3.1.11: icmp_seq=3 ttl=63 time=1.61 ms
64 bytes from 10.3.1.11: icmp_seq=4 ttl=63 time=0.910 ms
^C
--- 10.3.1.11 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3014ms
rtt min/avg/max/mdev = 0.838/1.146/1.612/0.305 ms
[root@server1 ~]#
```

#### 3. Comprendre le routage

🌞 Faites moi un 'tit tableau représentant cette trame choisie : 



| zizi | MAC src | MAC dst | IP src | IP dst |
| ---- | ------- | ------- | ------ | ------ |
| Dans net1 (trame qui entre dans router) |08:00:27:38:66:f6|08:00:27:9a:f6:01|10.3.1.11|10.3.1.254|
| Dans net2 (trame qui sort de router) | 08:00:27:89:0b:77 | 08:00:27:80:25:00|10.3.2.254|10.3.2.11|

### II. ARP

#### 1. Tables ARP

```
[root@server1 ~]# ip n
10.3.2.254 dev enp0s8 lladdr 08:00:27:89:0b:77 REACHABLE
10.3.2.1 dev enp0s8 lladdr 0a:00:27:00:00:1b DELAY
```

> L’hôte avec l’ip 10.3.2.254 est sur le domaine de diffusion dev enp0s8 la MAC a été obtenue via le protocole ARP et la trame en sera composée a chaque fois qu’un paquet est émis a destination de 10.3.2.254. REACHABLE indique que la connexion a été établie et que l'hôte est apparement joignable.

```
[root@router ~]# ip n
10.3.1.11 dev enp0s8 lladdr 08:00:27:38:66:f6 REACHABLE
10.3.1.1 dev enp0s8 lladdr 0a:00:27:00:00:0a DELAY
10.3.2.11 dev enp0s9 lladdr 08:00:27:80:25:00 REACHABLE
```

> L'hôte avec l'ip 10.3.2.11 est sur le domaine de diffusion dev enp0s9 la MAC a été obtenue via le protocole ARP et la trame en sera composée a chaque fois qu'un paquet est émis a destination de 10.3.2.11. STALE indique que la connexion a été établie mais le voisin n'est plus joignable.


```
[root@client1 ~]# ip n
10.3.1.1 dev enp0s8 lladdr 0a:00:27:00:00:0a REACHABLE
10.3.1.254 dev enp0s8 lladdr 08:00:27:9a:f6:01 REACHABLE
```

> L'hôte avec l'ip 10.3.1.1 est sur le domaine de diffusion dev enp0s8 la MAC a été obtenue via le protocole ARP et la trame en sera composée a chaque fois qu'un paquet est émis a destination de 10.3.1.1. DELAY indique qu'un paquet a été transmis et est en attente d'une réponse.


#### 2. Requêtes ARP

##### A. Table ARP 1

🌞 mettez en évidence le changement dans la table ARP de client1

- Avant

```
10.3.1.1 dev enp0s8 lladdr 0a:00:27:00:00:0a REACHABLE
```

- Apres 

```
10.3.1.1 dev enp0s8 lladdr 0a:00:27:00:00:0a REACHABLE
10.3.1.254 dev enp0s8 lladdr 08:00:27:9a:f6:01 REACHABLE
```
##### B. Table ARP 2

- Avant

```
10.3.2.1 dev enp0s8 lladdr 0a:00:27:00:00:1b REACHABLE
```

- Apres 

```
10.3.2.254 dev enp0s8 lladdr 08:00:27:89:0b:77 REACHABLE
10.3.2.1 dev enp0s8 lladdr 0a:00:27:00:00:1b REACHABLE
```

##### C. tcpdump 1

🌞 mettez en évidence toutes les trames ARP capturées lors de cet échange, et expliquer chacune d'entre elles

![](https://i.imgur.com/Lrw7dpp.png)

##### C. tcpdump 2

🌞 mettez en évidence toutes les trames ARP capturées lors de cet échange, et expliquer chacune d'entre elles

![](https://i.imgur.com/nITLzG6.png)

##### E. u okay bro ?

🌞 Expliquer, en une suite d'étapes claires, toutes les trames ARP échangées lorsque client1 envoie un ping vers server1, en traversant la machine router.

### Entracte : Donner un accès internet aux VMs

#### 🌞 Permettre un accès WAN (Internet) à client1

 - vérifiez que vous avez un accès au WAN (internet) avec les commandes :

 - - envoi d'un message simple vers un serveur en ligne

$ ping 8.8.8.8

```
[root@client1 ~]# ping 8.8.8.8
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=54 time=39.1 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=54 time=24.3 ms
64 bytes from 8.8.8.8: icmp_seq=3 ttl=54 time=31.8 ms
64 bytes from 8.8.8.8: icmp_seq=4 ttl=54 time=21.0 ms
^C
--- 8.8.8.8 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3007ms
rtt min/avg/max/mdev = 21.089/29.122/39.189/6.999 ms
```

 - - test de la résolution de nom DNS

$ dig google.com

```
root@client1 ~]# dig google.com

; <<>> DiG 9.11.4-P2-RedHat-9.11.4-9.P2.el7 <<>> google.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 58055
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1452
;; QUESTION SECTION:
;google.com.                    IN      A

;; ANSWER SECTION:
google.com.             218     IN      A       172.217.19.238

;; Query time: 34 msec
;; SERVER: 1.1.1.1#53(1.1.1.1)
;; WHEN: Mon Mar 16 17:59:07 CET 2020
;; MSG SIZE  rcvd: 55
```

### III. Plus de tcpdump

#### 1. TCP et UDP

##### A. Warm-up

- en UDP : 

```
[root@server1 ~]# nc -l 10.3.2.11 9999
hello world
[root@server1 ~]# 
```

```
[root@client1 ~]# nc 10.3.2.11 9999
hello world
^C
[root@client1 ~]# 
```

##### B. Analyse de trames

🌞 TCP : 

```
[root@client1 ~]# nc 10.3.2.11 9999
ça va la zone
parfait et toi
je suis en gucci
^C
[root@client1 ~]#
```

```
[root@server1 ~]# nc -l 10.3.2.11 9999
ça va la zone
parfait et toi
je suis en gucci
[root@server1 ~]#
```

 - Mise en évidence 3-way handshake tcp

![](https://i.imgur.com/2vQh3fW.png)

 - observez la suite des échanges (PSH, ACK, etc)

![](https://i.imgur.com/ITjMiAU.png)

 - observez la fin de connexion

![](https://i.imgur.com/KVgPlan.png)



---

🌞 UDP : 

```
[root@server1 ~]# nc -l 10.3.2.11 -u 9999
cc
cc toi
^C
[root@server1 ~]#
```

```
[root@client1 ~]# nc 10.3.2.11 -u 9999
cc
cc toi
^C
[root@client1 ~]#
```

![](https://i.imgur.com/C75diJa.png)

La différence que l'on remarque, c'est qu'il n'y a pas le "3-way handshake TCP"

Du coup, l'udp est plus rapide.

#### 2. SSH

Après s'être connecté en ssh sur notre serveur depuis le client. On observe la connection ssh dans wireshark. On remarque tout de suite que le protocole SSH utilise le TCP puisque qu'il n'y a aucune trame UDP.

### IV. Bonus

#### ARP cache poisonning. 

On part sur Arping

Sur notre client pirate on effectue cette commande : 

```
arping -c 1 -I enp0s8 10.3.1.11 (ip victime)
```

On va ensuite ping notre server 1 avec le client 1, et observer ce qui ce passe dans wireshark


![](https://i.imgur.com/w4mNnDD.png)


On voit un "duplicate use of 10.3.1.12 detected" ce qui veut dire que notre arp cache poisoning a marcher mais que wireshark l'a detecté avec un niveau de sécutité "WARINING"

#### NGINX

Apres avoir installé nginx on fait : 

```
systemctl start nginx && enable nginx
```

```
firewall-cmd --add-port=80/tcp --permanent
firewall-cmd --reload
```

On peut mettre notre ip serveur dans firefox et admirer le résultat : 

![](https://i.imgur.com/zKxp2p0.png)


# FIN DU TP3