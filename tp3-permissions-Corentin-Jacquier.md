
# 👨‍💻 TP2 - Permissions et users

CPE Lyon - 4ETI, 3IRC & 3ICS - Année 2022/2023 Administration Système

Corentin JACQUIER ICS

## Exercice 1 - Gestion des utilisateurs et des groupes

On ajoute les groupes avec la commande `sudo groupadd infra`  et de meme avec `dev`.

Pour ajouter les utilisateurs via bash sur le shell, on crée un fichier texte 'user.txt' qui contient le nom des utilisateurs (alice, bob, charlie, dave) avec `nano`. 

Puis avec un 'for' en bash on va exécuter la commande `sudo useradd -m [user]` pour chaque utilisteur contenu dans le fichier texte, avec la commande suivante:  `for i in 'cat user.txt' ; do sudo useradd -m -s /bin/bash $i ; done`. 

On utilise le paramètre `-m` pour créer le dossier personnel de l'utilisateur et `-s` pour spécifier le repértoire `/bin/bash`. 

On ajoute les utilisateurs à leurs groupes respectifs avec `sudo usermod -a -G [groupe] [user]`.

Voici une première méthode pour afficher les utilisateurs d'un groupe : on fait `grep 'infra' /etc/group`. En effet le fichier `/etc/group` contient la liste de tous les groupes et ses membres. 

<img src="https://cdn.discordapp.com/attachments/1017478318934724638/1021320957564030986/unknown.png">

La commande `getent group infra` permet également d'afficher les membres du groupe 'infra'. 

<img src="https://cdn.discordapp.com/attachments/1017478318934724638/1021322069994127490/unknown.png">

Permissions des personnes du répertoire avant :

<img src="https://cdn.discordapp.com/attachments/1017478318934724638/1021324004356792320/unknown.png">

Pour changer les permissions des `$home/[user]` ajoutés, on fait `sudo chgrp [groupe] [dossier home]` dans le dossier `$home`.  Alors, on obtient : 

<img src="https://cdn.discordapp.com/attachments/1017478318934724638/1021326067669159946/unknown.png">

Pour changer le groupe primaire d'un utilisateur, on fait `sudo usermod -g [Groupe primaire] [User]`, donc par exemple pour Alice on fait `sudo usermod -g dev alice`.

Avec la commande `sudo mkdir`, on ajoute les répertoires `/home/infra` et `/home/dev`.

Et on ajoute les permissions aux groupes respectifs avec `sudo chgrp dev dev` et `sudo chgrp infra infra` .

On a donc :

<img src="https://media.discordapp.net/attachments/1017478318934724638/1021330458698579968/unknown.png">

On active le sticky bit pour empêcher de renommer et supprimer les fichiers avec  `chmod +t /home/dev` et `chmod +t /home/infra`

Il est impossible de se connecter au compte Alice avec la commande `su alice`. En effet, il faut d'abord mettre un mot de passe sur ce compte avec `sudo passwd alice`, alors son compte sera activé. 

On peut donc ouvrir une session depuis le compte root avec l'utilisateur Alice :  `su alice`.

C'est avec les commandes `id -u alice` on obtient l'uid et  `id -g alice` on obtient l'gid. 

<img src="https://media.discordapp.net/attachments/1017478318934724638/1021334795676024842/unknown.png">

Les deux id sont `1002`.

Avec la commande `id -nu 1003`, on trouve à qui appartient l'uid `1003` soit Bob.

Avec la commande `getent group dev`, on trouve que le le gid du groupe est `1002` accompagné de la liste des membres du groupe. 

Et pour avoir le nom d'une groupe en fonction d'un gid, on fait `getent group [gid]`.

Pour supprimer un membre d'un groupe, on exécute la commande `sudo gpasswd -d charlie infra`. 

Charlie ne fait plus partie du groupe infra (et n'a plus accès au répertoire `infra`).

On utilise la commande `chage dave` pour éditer les paramètres d'expiration du mot de passe de Dave. 

D'après la commande `sudo cat /etc/user`, le root utilise `/bin/bash`.

L'utilisateur `nobody` (qui est dans le groupe `nobody`), possède le minimum de permissions possible. 

La commande `sudo` garde en mémoire le mot de passe pendant 15 min, pour forcer la demande de mot de passe on utilise `sudo -k`. 

## Exercice 2 - Gestion des permissions

Puis, on affiche les permissions avec la commande `ls -l`, on peut observer que le dossier 'test' possède les droits suivants : `drwxr-xr-x`. Et les droits du fichier fichier sont : `-rw-r--r--`.

Avec la commande `sudo chmod 000 fichier`, on enlève toutes les permissions du fichier. Mais l'utilisateur root peut encore modifier et afficher le fichier. 



