Pourquoi ?
==========

-   Apache va devenir petit à petit la norme avec Jeedom

-   Permet d’avoir les dernières mises à jour de sécurité au niveau de
    l’accès aux fichiers (grâce au .htaccess) lors des mises à jour de
    Jeedom

-   Corrige certains soucis d’accès et de stabilité (notamment pour ceux
    qui ont beaucoup de caméras)

-   Améliore les performances globales de Jeedom.

Prérequis
=========

-   Savoir se connecter en SSH sur la box (vous trouverez les
    identifiants sur la documentation d’installation)

-   Etre connecté à internet.

> **Important**
>
> En cas de soucis l'équipe Jeedom ne pourra être tenue responsable et
> pourra refuser toute demande de support. La manipulation est à vos
> risques et périls.

> **Important**
>
> Attention certains plugins non officiels ne sont pas compatibles
> Apache, renseignez-vous bien avant.

Comment faire
=============

Désactivation des services Jeedom et nginx
------------------------------------------

Dans Jeedom, il faut aller sur chaque plugin ayant un démon, puis
désactiver la gestion automatique du démon et couper celui-ci. Il faut
ensuite, dans le moteur de tâches, désactiver toutes les tâches (il y un
bouton de désactivation générale) et dans les scénarios, désactiver tous
les scénarios (il y un bouton de désactivation générale).

    systemctl stop cron
    systemctl stop nginx
    systemctl stop mysql

Installation et configuration d’Apache
--------------------------------------

    mkdir -p /var/www/html/log
    apt-get -y install ntp ca-certificates unzip curl sudo
    apt-get -y install apache2 php5 mysql-client mysql-server libapache2-mod-php5
    apt-get -y install php5-cli php5-common php5-curl php5-fpm php5-json php5-mysql php5-gd
    wget https://raw.githubusercontent.com/jeedom/core/stable/install/apache_security -O /etc/apache2/conf-available/security.conf
    rm /etc/apache2/conf-enabled/security.conf
    ln -s /etc/apache2/conf-available/security.conf /etc/apache2/conf-enabled/
    rm /etc/apache2/conf-available/other-vhosts-access-log.conf
    rm /etc/apache2/conf-enabled/other-vhosts-access-log.conf
    systemctl restart apache2
    rm /var/www/html/index.html

> **Note**
>
> Si lors de l’installation le système vous demande si vous voulez ou
> non garder une version modifiée d’un fichier, faites : "Keep the local
> version currently installed".

Copie de Jeedom
---------------

    cp -R /usr/share/nginx/www/jeedom/* /var/www/html/
    rm /var/www/html/log/nginx.error
    chmod 775 -R /var/www/html
    chown www-data:www-data -R /var/www/html

Test d’accès
------------

    systemctl start mysql

Vous devriez maintenant pouvoir accéder à Jeedom depuis la même URL
qu’avant. Si c’est bon vous pouvez continuer SINON IL NE FAUT SURTOUT
PAS FAIRE LA SUITE.

Mise à jour de la crontab
-------------------------

Faire :

    crontab -e

Puis mettre à jour le chemin d’accès à Jeedom, remplacer :

    * * * * * su --shell=/bin/bash - www-data -c '/usr/bin/php /usr/share/nginx/www/jeedom/core/php/jeeCron.php' >> /dev/null 2>&1

Par :

    * * * * * su --shell=/bin/bash - www-data -c '/usr/bin/php /var/www/html/core/php/jeeCron.php' >> /dev/null 2>&1

Nettoyage et suppression de nginx
---------------------------------

    apt-get remove nginx*
    rm -rf cp -R /usr/share/nginx
    apt-get autoremove
    systemctl disable nginx

Redémarrage des services
------------------------

    systemctl enable apache2
    systemctl start cron

Ensuite connectez-vous à votre Jeedom et réactivez le moteur de tâches
et les scénarios. Vous pouvez aussi relancer les démons.

> **Important**
>
> Il est conseillé après la migration de lancer une mise à jour de
> Jeedom (même s’il ne vous propose rien).

