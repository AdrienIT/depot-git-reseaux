# Sujet 2 : Débugger et désassembler des programmes compilés

## Exercices

### Hello World

→ Code Hello World en C : 

```
#include <stdio.h>
int main() {
   printf("Hello, World!");
   return 0;
}
```

Maintenant on va venir désassembler notre program pour vir où est stocké la chaine de charactère.

Je reverse avec radare2.

```
Liste des commandes :
r2 hello
s main
v
```

![](https://i.imgur.com/HNSssh5.png)

Arrivé ici, en étudiant un peu plus, on remarque ceci : 

![](https://i.imgur.com/J6pAvit.png)


La string "Hello, World!" est stockée à l'adresse : 0x2004

### Winrar crack

Ma version de WinRar : 5.40

J'utiliserais x64dbg et RessourceHacker.

#### Step 1 - Que changer ?

On va venir run winrar avec x64dbg.

Donc en le lançant, on remarque ça : 

![](https://i.imgur.com/c364tZe.png)

Et si on changait ça ?

#### Step 2 - Trouver l'adresse de la string.

Let's go ouvrir RessourceHacker.

On cherche "evaluation copy" avec ctrl + f

![](https://i.imgur.com/DaQJFhl.png)


873 = "evaluation copy"

872 = "Correct registration"

#### Step 3 - Reperer, dans x64dbg où est la string.

On vient chercher dans le module, l'adresse 873 : 

![](https://i.imgur.com/27pAg5R.png)

Et voilà 

![](https://i.imgur.com/WFVUdIS.png)

Il faut savoir que 369 est la valeur hexadécimal de 873.

L'instruction mov ecx, 369 veut dire : copie la valeure 369 au registre ecx.

#### Step 4 - Changer la string

On se rappelle que Ressource Hacker nous a donné ça : 

872 = "Correct registration"

Let's go double clique dessus, on va changer la valeur 369 à 368. (368 = 872 en hexadecimal).

![](https://i.imgur.com/PSFwqfE.png)

Et boom, on passe en "Correct registration"