#### Cours d'administration et supervision reseaux

***(18/08/2022)***

:::tip	Important
## Quels sont les profils d'acces d'une machine windows a un reseaux ?

*	** Public: ** Partages desactives, ping bloque
*	** Prive: ** Partages actives
*	** Domaine: ** Les regles de securites sont gerer au niveau du controleur de domaine
:::

L'outil loupe est utiliser pour zoomer sont ecran.

:::info Information
Pour ouvrir le firewall on tape firewall dans la barre de recherche de windows. Cela nous permet aussi de voir les profils d'acces.
:::

## Comment autoriser a travers un parefeu une application windows ?

# Dans le parefeu windows la fenetre a gauche nous pouvons autoriser une application.


## Est-ce que ces deux machines sont dans le meme reseaux:
*	W: 192.168.3.97
*	L: 192.168.5.245
?

## On ne peut pas se prononcer tant qu'on a pas le masque reseau

Avec comme information supplementaire le masque:
- W: 192.168.3.97/21
- L: 192.168.5.245/21

On fait le calcule binaire de l'application des addresses au masque reseau et on trouve qu'ils sont dans le meme reseau.

## Interdire le ping sur linux

On passe en mode super utilisateur

Ensuite on execute la commande:

``` jsx title="bash"
iptables -A input -p icmp -j REJET
```

### ***Que faut-il maitriser dans le cadre de ce cour ? ( 24/08/2022 )***

### Gestion des equipements que RADIO

## Fonctionnement de l'environnement de travail (windows, linux)

## Fonctionnement des cartes reseaux

# Mode promiscuite:
Il permet a une carte reseaux d'accepter toutes les trames qui lui sont envoyer

# Machine virtuelle:
Pour permettre a notre machine virtuelle d'etre dans le meme reseaux que

# net-tools

``` jsx title="bash"
ifconfig
```
Permet de voir nos cartes reseaux

``` jsx title="bash"
iwconfig 
```
Permet de voir les cartes Wi-Fi

``` jsx title="bash" 
iwlist <nom de la carte>
```
Permet de lister les canaux supporter par la machine

:::info Information
Une bande de frequence a ete laisse pour les gens qui fabriquent des equipements pour les industries les scientifiques et pour le medical. Cette bande s'appelle ISM.
ISM: Industrie Scientifique Medical ( 2 bandes ISM: 2,4 GHz et 5 GHz )
:::

``` jsx title="bash"
iwlist <nom de la carte> scan
```
Pour afficher les Wi-Fi disponible et les informations dessus.

Si Mode == "Master" (Insfrastrure) alors ce reseau est un point d'acces

Si Mode == "" (Point a point) alors ce reseau est un point a point

:::info Information
En Wi-Fi une frequence occupe 11MHz a gauche et 11MHz a droite donc il est conseiller d'utiliser une ecart de 5 unites ( MHz ) lorsque deux finis sont proches.
La premiere frequence est: 2,412 GHz
***La formule pour calculer la frequence de la ieme canal: fi = f1 + (i-1) * 0,005***
:::

## Le fonctionnement d'un switch:
*	table de commutation
*	Algo de reception de trames pour un switch

:::info Information
*	Port mirroring ( Le fait de copier toutes les trames envoyer sur les autres ports et les envoyer sur un autre port qui est port ou je veux placer une outils de supervision )
*	Port forwarding
:::

:::tip Contraste 

***Lien entre les concepts entre le fonctionnement de la carte reseaux et les concepts de port mirroring et port Forwarding***

Promiscuite: La trame va accepter toutes les trames memes celle qui ne lui sont psa desiner

Port mirroring:

:::

# Pare-feu et filtrage sous linux

Les deux outils qu'on va utiliser:
*	iptables
*	ufw

Pour lister les regles de securite `iptable -L`

:::tip Important
Le noyau du SE fabrique les datagrammes ip. Lorsqu'un routeur recoit une datagramme ip il diminue le champt *TTL* d'une unite. Lorsque ce champs atteint zero ce datagramme est detruit.
:::

Ajouter une regle qui bloquer tous les ping:
``` jsx title="bash"
iptables -A INPUT -p icmp -j REJET
```

Supprimer toutes les regles:
``` jsx title="bash"
iptables -F
```

Ajouter une regler qui bloque tous les ping sauf pour une machine:
``` jsx title="bash"
iptables -A INPUT -p icmp ! -S 192.168.7.25 -j REJET
```

Pour afficher l'etat du service ufw:
``` jsx title="bash"
systemctl status <nom du service>
```

Pour activer ou desactiver une service:
``` jsx title="bash"
systemctl enable/disable <nom du service>
```

Pour masquer une service:
``` jsx title="bash"
systemctl mask/unmask <nom du service>
```

:::tip Cas d'utilisation de `mask/unmask`
Si on doit mettre en place un serveur de telephonie sur ip et qu'on utlise *Asterisk* et *FreeSwitch* on peut masquer un service pendant qu'on travaille sur l'autre et apres inverser pour qu'ils n'utilisent pas les memes ports.
:::

# Outils d'analyse de trames:
*	Wireshark
*	Telnet

:::tip Installation
*	Wireshark
`sudo apt install wireshark`

*	Telnet
`sudo apt install telnetd`
:::

:::info Information
Sous linux le "d" a la fin du nom d'un paquet signifie que c'est un paquet serveur. Il existe aussi son omologue sans le "d" qui signifie que c'est le paquet client.
:::

:::info Information
La commande: `apt policy <nom du paquet>` est utiliser pour savoir si le paquet est installer.
:::

Pour afficher les connexions réseau, les tables de routage, les statistiques des interfaces, les connexions masquées, les messages netlink, et les membres multicast. On filtre les avec les lignes contenant "23" ( Port de telnet )
``` jsx title="bash"
netstat -anp | grep -w 23
```


:::tip Pour qu'un client puisse acceder a un service
*	Adresse IP du serveur
*	Port de serveur
*	Potrocole de transport
:::

:::danger Important !
*	Si un protocole utlise udp, c'est qu'il n'est pas sensible au erreur.
*	Si un protocole utilise tcp, c'est qu'il est sensible au erreur. 
:::

Pour installer le serveur ssh:
``` jsx title="bash"
sudo apt install openssh-server
```

Pour se connecter avec ssh sur une machine:
``` jsx title="bash"
ssh <nom d'utilisateur>@<adresse de la machine>
```

:::tip Important
Pour savoir qui est connecter sur la machine: `who`

Pour savoir qui fait quoi sur la machine: `w`
:::

**Wireshark**

![Interface de wireshark](./ad-00001.png)


## I- Principes de mise ne place d'un service reseaux ***(25/08/2022)***

# 1- Comprendre l'interet du service

:::info Exemple
*	Le service Telnet permet la connexion a distance

*	Le service SSH permet la connexion a distance securse
:::

# 2- Connaitre les entites du service

# 3- Connaitre les protocoles du service

# 4- Connaitre les differents types de messages echanges par les entites du service

# 5- Connaitre le format de chaque message

# 6- Intallation selon le cas:

*	Windows Server

:::info Que faire ?

*	On active: le service
*	On parametre graphiquement (Connaitre les principaux parametres du service et leur signification)
*	Apprendre a utiliser le service

:::

*	Linux

:::info Que faire ?

*	Connaitre les noms des paquets a installer
*	On installe les paquets(\*)


***3 grandes methodes:***

*	M1: Le paquet est connu par la distribution
*	M2: Le paquet est archive
*	M3: 
	-- Installation de la cle du logiciel;
	-- Installer le lien de telechargement du logiciel
	-- `apt update ; apt install <nom du logiciel>`
:::

# 7- Configuration selon:

*	Linux: Identifier les fichiers de configuration, identifier les parametres du service, parametrer et redemarrer le service.


### Pratique

# 

Pour qu'un utilisateur deja creer puisse utiliser la commande `sudo` il faut l'ajouter dans le groupe sudo avec la commande:
``` jsx title="bash"
gpasswd -a <nom d'utilisateur> sudo
```

Pour supprimer un utilisateur d'un groupe on fait:

``` jsx title="bash"
gpasswd -d <nom d'utilisateur> <nom du groupe>
```

Pour nommer un utiilisateur administrateur d'un groupe:
``` jsx title="bash"
gpasswd -A <nom d'utilisateur> sudo
```

## Analyse des trames

*	Se familialiser avec l'encapsulation
*	Sensibiliser les apprenants sur les problemes de securite de certaines applications
*	Se familialiser avec certains services reseaux (Telnet, SSH, mysql, FTP, messagerie, DNS, DHCP, TFTP, LDAP, ...)

:::info Remarque !

Sous linux, un service peut etre autonome ou non autonome. Si un service est non-autonome il est gerer par le super deamon`init`

:::

:::tip Important
On peut trouver tous les services confier on regarde le fichier: `/etc/inetd.cnf`
:::

# Telnet

Il n'est pas securise, il fonctionne en mode caractere.

# SSH

Il est securise

# FTP

Il fonctionnee en mode message

:::tip Clients
filezilla & winscp
:::

:::info Information
On se pose deux questions:
*	Autoriser les utilisateurs anonymes a se connecter ?
*	Faut-il autoriser les utilisateurs ayant un compte a se connecter ?
*	Faut-il bloquer l'utilisateur dans son dossier personnel ?
:::

Le fichier de configuration du serveur: `/etc/vsftpd.conf`

:::danger Parametres

``` jsx title="bash"
anonymous\_enable=NO
local\_enable=YES
write\_enable=YES
#chroot\_local\_user=YES
```
:::

## Exercice:

Confiner un utilisateur dans son repertoire personnel avec FTP (vsftpd)

## Installation et utilisation du client ftp filezilla

*	sudo apt install filezilla
*	Lancer filezilla

:::info Conclusion
FTP n'est pas securise
Le protocol FTP
:::

# Exercice

Creer un tunnel ssh pour y faire passer des messages les messages services non securises.

# mysql

:::info Paquets
Client: mysql-client
Serveur: mysql-server
:::

:::tip Fichier de configuration
`/etc/mysql/mysql.conf.d/mysqld.cnf`
:::

Pour que notre service de base de donnees soit accessible par tous:
*Parametre:* bind-address =	0.0.0.0

Pour activer les logs:
*Parametre* 
*	general\_log\_file        = /var/log/mysql/query.log
*	general\_log             = 1

:::tip Commandes
``` jsx title="mysql"
show databases;
create database master;
create user bobo identified by "P@sser123";

grant all privileges on master.* to bobo;
flush privileges;
```
:::

Pour voir les logs, nous fesons un tail sur le fichier:
`/var/log/mysql/mysql-bin.log`



On peut faire 

``` jsx title="mysql"
create table etudiant(id int primary key AUTO_INCREMENT, prenom varchar(30), nom varchar(30), note varchar(30));

insert into etudiant(prenom, nom, note values("Cherif", "Diouf", "18"), ("Kahar", "Idrissou", "18");

```

# Apache2

``` jsx title="bash"
sudo apt install apache2
```

:::tip Dossier site
`/var/www/html/`
:::


On creer un fichier /var/www/html/test/bonjour.php avec le contenue:
``` jsx title="php"

<?php 
	echo "Bonjour les amis";
	echo "<br>" // On fait semblant de faire une erreur au niveau de cette ligne
	echo "Bonjour de nouveau";
?>
```


:::danger Important !

Pour qu'apache puisse afficher du php il faut installer le module: `libapache2-mod-php`

:::


:::info Logs d'apache2

Le ficher log c'est le:
`/var/log/apache2/error.log`

:::


:::tip Logs
	Pour regarder les fichiers de log on utilise tail -f suivis du nom du fichier pour voir la fin
:::



















