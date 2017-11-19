Shellinabox est une console accessible depuis un navigateur.

Cela se présente sous la forme d’un service écoutant sur le port 4200 en
https. On va décrire son installation et ensuite les configurations de
reverse proxy

Installation de Shellinabox
===========================

Voici la commande à lancer pour installer Shellinabox :

    sudo apt-get -y shellinabox

Reverse Proxy Apache
====================

Il faut créer un fichier dans
/etc/apache2/sites-enabled/shellinabox.conf avec le contenu ci-dessous
et ensuite redémarrer Apache

    <Location /jeedom/shellinabox>
    Proxypass  http://localhost:4200/
    #ProxyPassReverse  http://localhost:4200/
    Allow from all
    </Location>

Reverse Proxy Nginx
===================

Il faut créer un fichier dans /etc/nginx/sites-enabled/shellinabox.conf
avec le contenu ci-dessous et ensuite redémarrer Nginx

    location /jeedom/shellinabox/ {
    proxy_pass http://127.0.0.1:4200;
    proxy_set_header Host $host;
    proxy_buffering off;
    tcp_nodelay on;
    access_log off;
    }
