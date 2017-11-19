Nous allons voir ici comment faire une mise à jour manuellement sur
Jeedom.

Prérequis
=========

-   savoir se connecter en SSH à Jeedom

-   connaître les identifiants SSH (voir documentation d’installation)

-   avoir internet sur la box Jeedom

> **Important**
>
> Pensez bien à faire et récupérer un backup avant toute mise à jour
> manuelle.

Téléchargement et décompression
===============================

En SSH, faites :

    sudo su -
    cd /root
    wget https://github.com/jeedom/core/archive/stable.zip
    unzip stable.zip
    cp -R core-stable/* /var/www/html

Passage des mises à jour
========================

Toujours en SSH:

    sudo su -
    php /var/www/html/install/update.php mode=force
    chmod 775 -R /var/www/html
    chown www-data:www-data -R /var/www/html

> **Important**
>
> Si votre installation de Jeedom est un peu ancienne, il faut remplacer
> tous les /var/www/html par /usr/share/nginx/www/jeedom

