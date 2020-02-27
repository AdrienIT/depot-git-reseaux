# TP 2 : Adrien Zoghbi

## TP 2 - Machine virtuelle, réseau, serveurs, routage simple

## I. Création et utilisation simples d'une VM CentOS

### 4. Configuration réseau d'une machine CentOS

 - Déterminer la liste de vos cartes réseau, les informations qui y sont liées, et la fonction de chacune.

| Name | IP | MAC | Fonction |
| -------- | -------- | -------- | -------- |
| enp0s3 | 10.0.2.15     | 08:00:27:0e:9d:cf | -------- |
| enp0s8 | 10.2.1.2     | 08:00:27:72:18:7d | -------- |

 - Changer la configuration de la carte réseau Host-Only

Changeons l'ip de la carte réseau enp0s3

Avant : 
![](https://i.imgur.com/yPwISA9.png)

Après : 
![](https://i.imgur.com/bsMMRam.png)

Ping de ma vm vers mon ordi : 
![](https://i.imgur.com/mt6wdOL.png)

### 5. Appréhension de quelques commandes

-  Faites un scan nmap du réseau host-only




- Utiliser ss pour lister les ports TCP et UDP en écoute sur la machine

Après avoir effectué la commande "ss -ltunp" on remarque que le protocol sshd tourne en écoute sur le port 22

![](https://i.imgur.com/TKMhezf.png)

## II. Notion de ports

### 1. SSH

Grace à la commande : 
```
ss -ltunp
```
On remarque que le port ssh écoute sur le 22.

Pour se connecter en ssh depuis notre PC hote, il faut faire cette commande dans notre terminal : 
```
ssh root@10.2.1.1 -p 22
```

Preuve de la connexion : 
![](https://i.imgur.com/p8CJfOD.png)

### 2. Firewall

#### A. SSH
 - Modifier le port du service SSH

```
nano /etc/ssh/sshd_config
```
Et on on modifie la ligne "Port 22" pour mettre 2220

Il faut ensuite ajouter la règle dans le firewall

Pour ce faire : 
```
sudo firewall-cmd --add-port=2220/tcp --permanent
```

```
sudo firewall-cmd reload
```
On peut ensuite se reconnecter en ssh sur notre magnifique port 2220 depuis notre PC HOTE : 

```
PS C:\Users\Adrien>ssh root@10.2.1.2 -p 2220
root@10.2.1.2's password:
Last login: Thu Feb 20 14:02:04 2020 from 10.2.1.1
[root@patron ~]#
```

#### B. Netcat

- Comme dans le précédent TP, on va faire un ptit chat. La VM sera le serveur, le PC sera le client.

On fait de notre VM, notre serveur : 
```
nc -lp 4444
```
On ouvre le port 4444
```
sudo firewall-cmd --add-port=4444/tcp --permanent
```

```
sudo firewall-cmd reload
```
Notre serveur est maintenant ouvert.
Depuis notre PC HOTE, il faut se connecter à notre serveur : 
```
nc 10.2.1.2 4444
```
![](https://i.imgur.com/iDhF99B.png)
Boom ça marche.
Et pour voir la connexion sur notre serveur, il suffit de connaitre un peu les jobs linux et faire : 
CTRL-Z 
![](https://i.imgur.com/vOjlQur.png)

On peut maintenant faire nos commandes (dont celle pour voir l'interaction entre notre VM et notre PC HOTE) : 

![](https://i.imgur.com/gTW7krP.png)

Et voila.

- Inversez les rôles, le PC est le serveur netcat, la VM est le client.

Pour faire l'inverse c'est simple, on effectue les commandes de la VM sur notre PC HOTE et les commandes de notre PC HOTE sur notre vm : 

En effectuant notre ouverture de port, on a une notif windows : 
![](https://i.imgur.com/iifwZQH.png)

C'est la notif pour notre regle firewall, bref ...

On effectue la commande pour se connecter : 
```
nc 10.2.1.1 4444
```

![](https://i.imgur.com/oYRTnwr.png)

#### 3. Wireshark

On lance wireshark, on enleve internet.

On envoie un message dans le netcat.

Et on observe wireshark

![](https://i.imgur.com/M7iGQIn.png)

On obtient nos 2 trames (Envoie et réponse)

Le message est bien visible : 
![](https://i.imgur.com/zaIv64E.png)

Les messages échangés sont : 
SYN
SYNACK
ACK
![](https://i.imgur.com/kgYOJVC.png)

## III. Routage statique

### 1. Préparation des hôtes (vos PCs)

#### Check

- PC1 et PC2 se ping en utilisant le réseau 12

-> ![](https://i.imgur.com/wgI8Zza.png)


- VM1 et PC1 se ping en utilisant le réseau 1

-> ![](https://i.imgur.com/9qcoA3O.png)

- VM2 et PC2 se ping en utilisant le réseau 2

-> 

- Activation routage 

-> ![](https://i.imgur.com/IXOxYRo.png)

### 2. Configuration du routage

#### A. PC1

 - PC1 accède déjà aux réseaux 1 et 12, il faut juste lui dire comment accéder au réseau 2

(Sous windows)

```
route add 10.2.2.0/24 mask 255.255.255.0 10.2.12.2
```
```
OK !
```

- :sun_with_face: PC1 devrait pouvoir ping 10.2.2.1 (l'adresse de PC2 dans 2)

-> En effet : ![](https://i.imgur.com/wgI8Zza.png)

#### B. PC2

(Sous Linux)
```
ip route add 10.2.2.0/24 via 10.2.12.2 dev eth0
```
```
OK !
```
- :sun_with_face: PC2 devrait pouvoir ping 10.2.1.1 (l'adresse de PC1 dans 1).

-> En effet ![](https://i.imgur.com/usLZiWt.png)

#### C. VM1

- 🌞 Il faut dire à VM1 qu'elle peut joindre le réseau 2 en utilisant le lien qui l'unit avec PC1.

```
sudo ip route add 10.2.2.0/24 via 10.2.1.1 dev enp0s8
```

(On est sur windows, faut désactiver le firewall)

 - VM1 devrait pouvoir ping 10.2.2.1, l'adresse de PC2 dans le réseau 2.

Et oui, ça work : 

![](https://i.imgur.com/1up9tGx.png)

Traceroute : 

![](https://i.imgur.com/0G6RReV.png)




#### D. VM2

- 🌞 VM2 devrait pouvoir ping 10.2.1.1, l'adresse de PC1 dans le réseau 1.

```
sudo ip route add 10.2.1.0/24 via 10.2.2.1 dev enp0s8
```

Et ça marche aussi :)

![](https://i.imgur.com/AybqEsb.png)

Traceroute : 

![](https://i.imgur.com/nu0k4QU.png)



#### E. El gran final

- 🌞 VM1 et VM2 se ping.

![](https://i.imgur.com/Pl7tdOP.png)

### 3. Configuration des noms de domaine

Après avoir modifier les fichiers hosts, on ajoute des noms de domaines et on peut désormais ping en remplaçant l'ip de la vm par : vm2.tp2.b1

🌞 utilisation des noms de domaines

-> ![](https://i.imgur.com/drpkihL.png)

Traceroute

-> ![](https://i.imgur.com/36fkfmy.png)

- Netcat : 

Vm1 : serveur

Vm2 : client

![](https://i.imgur.com/HkAB7sY.png)

Et ça marche. 

FIN DU TP2