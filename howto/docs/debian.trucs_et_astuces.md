Paquets utiles
==============

Voici quelques paquets utiles à mettre sur une installation vierge :

    apt-get install -y vim fail2ban net-tools dos2unix

Si vous êtes sur VMware :

    apt-get install -y open-vm-tools

Ajout d’un bashrc coloré
========================

    rm -rf /root/.bashrc
    wget https://raw.githubusercontent.com/jeedom/core/stable/install/bashrc -O /root/.bashrc
    dos2unix /root/.bashrc

Autoriser la connexion root en SSH
==================================

Il faut éditer le fichier /etc/ssh/sshd\_config et changer :

    PermitRootLogin without-password

Par :

    PermitRootLogin yes

Monter un partage Samba
=======================

Installation du paquet cifs

    apt-get install -y cifs-utils

Créer le point de montage :

    mkdir /mnt/mon_partage

> **Note**
>
> Il faut adapter mon\_partage en fonction de votre besoin

Ajout du montage dans /etc/fstab

    //IP_SERVER_SAMBA/mon_partage /mnt/mon_partage cifs uid=0,rw,user=TODO,password=TODO 0 0

> **Note**
>
> Vous devez changer les TODO pour user et password par rapport à votre
> configuration

Passage de Jessie à Stretch
===========================

Pour avoir testé l’upgrade et l’installation Stretch avec restauration
d’un backup, je confirme que l’installation de Stretch par écrasement
vous fera gagner du temps.

-   Méthode 1 : une demi-journée à essuyer les bugs.

-   Méthode 2 : 1 a 2 heures grand max, et surtout un os cleaned.

Méthode 1 : Installation sur Strech et restauration de sauvegarde
-----------------------------------------------------------------

Avant de commencer, réaliser une sauvegarde complète via Jeedom de votre
installation sous Jessie, puis exporter la sauvegarde sur un autre
support de stockage.

[CONSEIL] Télécharger la sauvegarde autrement que par l’interface web
(ssh, ftp, samba, autres de votre choix), car si votre archive est
volumineuse elle peut facilement se corrompre via un téléchargement
Http. Si elle fait moins de 100Mo, c’est jouable.

-   Installer Debian Strech sur votre box.

-   Reconfigurer votre réseau local, vérifier que votre machine est
    opérationnelle et à jour.

-   Installer Jeedom en suivant la doc :
    <https://github.com/jeedom/documentation/blob/master/installation/fr_FR/other.asciidoc>

[ATTENTION] MariaDB n’autorise plus l’accès au profil *root*, ce qui
peut bloquer la restauration d’une base de donnée dont vous auriez
changé le nom (comme moi) donc on ne restaure pas tout de suite le
backup. Si l’utilisateur *jeedom* n’a pas les bonnes permissions, la
restauration échoue.

Référence :
<http://jc.etiemble.free.fr/abc/index.php/realisations/trucs-astuces/deb9php7>
(chapitre 5a)

En bref, 2 lignes de commande pour autorisé l’utilisateur *root* dans
mysql sous Stretch :

    mysql -u root -p mysql
    Enter password:
    Welcome to the MariaDB monitor.  Commands end with ; or \g.
    Your MariaDB connection id is 2
    Server version: 10.1.21-MariaDB-5 Debian 9.0
    Copyright (c) 2000, 2016, Oracle, MariaDB Corporation Ab and others.
    Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

    MariaDB [mysql]>
    MariaDB [mysql]> GRANT ALL PRIVILEGES ON *.* TO root@'localhost' IDENTIFIED BY 'monpass';
    Query OK, 0 rows affected (0.00 sec)
    MariaDB [mysql]> exit;
    Bye

[NOTE] remplacer *monpass* par votre mot de passe MYSQL utilisé pour le
compte root sous "Debian 8 - Jessie". Je donne les droits a root
notamment pour gérer mes bases avec *PHPMYADMIN*, mais les donner à
*jeedom* doit suffire.

A vous d’adapter cette commande en fonction de votre configuration
précédente :

    GRANT ALL PRIVILEGES ON *.* TO root@'localhost' IDENTIFIED BY 'monpass';
    ou
    GRANT ALL PRIVILEGES ON *.* TO jeedom@'localhost' IDENTIFIED BY 'monpass';
    (Remplacer 'monpass' par le mot de passe disponible dans Jeedom > Configuration > OS/DB > Base de données)

-   Copier votre sauvegarde dans le dossier /var/www/html/backup

-   Donner les droits à www-data : chown -R www-data:
    /var/www/html/backup/\*

-   Lancer la restauration via l’interface de Jeedom (Sauvegardes \>
    Sauvegardes Locales)

-   Patienter pendant la restauration

-   Redonner les droits à www-data sur tout jeedom

<!-- -->

    chown -R www-data: /var/www/html/

-   Redémarrer la box

-   Connexion à Jeedom avec vos anciens identifiants

-   Passer sur chaque plugin pour réinstaller les dépendances (notamment
    sur ceux ou le daemon est "NOK" KO).

Méthode 1 : Upgrade (peu de chance de succès)
---------------------------------------------

Mise à jour de l’OS en version jessie.

    apt-get -y update
    apt-get -y upgrade
    apt-get -y dist-upgrade

Il faut éditer le fichier /etc/apt/sources.list et remplacer tous les
Jessie par Stretch, puis faire :

    cp /etc/apt/sources.list /etc/apt/sources.list_backup
    sed -i 's/jessie/stretch/g' /etc/apt/sources.list

Mise à jour de l’OS en version stretch.

    apt-get -y update
    apt-get -y upgrade
    apt-get -y dist-upgrade

Bascule en MariaDB.

    apt-get -y install mariadb-server mariadb-client mariadb-common

Mise à jour de Jeedom

    sh /var/www/html/install/install.sh -s 2
    sh /var/www/html/install/install.sh -s 5
    sh /var/www/html/install/install.sh -s 7
    sh /var/www/html/install/install.sh -s 10

Suppression des librairies non nécessaires

    apt -y remove `aptitude -F %p search '~o' | grep -E -v ^lib`
    apt -y remove `aptitude -F %p search '~o'`----
