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
