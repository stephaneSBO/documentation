Nodered est un outil écrit en Nodejs pour gérer les flux orientés IoT ou
domotique. Il propose une interface graphique pour éditer des flows. Le
tuto décrit son installation, une configuration reverse proxy et un
démarrage en service

Installation de Nodered
=======================

Voici les commandes à lancer pour installer nodered en ayant un nodejs
fonctionnel :

    sudo apt-get -y install libavahi-compat-libdnssd-dev libusb-1.0-0-dev build-essential
    sudo npm install -g node-red

Pour obtenir Nodejs vous pouvez installer un des plugins jeedom qui s’en
charge.

Démarrage automatique de Nodered
================================

Il est possible de déclarer Nodered comme un service pour qu’il soit
démarré automatiquement au boot de la box. Un exemple ici :

<https://gist.github.com/bigmonkeyboy/9962293>

Configuration d’Apache
======================

Voici un exemple de fichier pour avoir accès à Nodered via un reverse
proxy.

    <Location /jeedom/nodered>
    ProxyPass ws://localhost:1880/jeedom/nodered/
    ProxyPass http://localhost:1880/jeedom/nodered/
    #ProxyPassReverse ws://localhost:1880/jeedom/nodered/
    #ProxyPassReverse http://localhost:1880/jeedom/nodered/
    </Location>

Configuration de Nginx
======================

Voici un exemple de fichier pour avoir accès à Nodered via un reverse
proxy.

    location /jeedom/nodered/ {
      proxy_pass http://127.0.0.1:1880;
      proxy_set_header Host $host;
      proxy_buffering off;
      tcp_nodelay on;
      access_log off;
      proxy_http_version 1.1;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection 'Upgrade';
      proxy_redirect off;
      proxy_read_timeout 6000;
    }

Modules existants pour Nodered
==============================

Voici des exemples d’extensions disponibles pour Nodered.

Avahi/Bonjour discovery module
==============================

sudo npm install node-red-node-discovery -g \# google module sudo npm
install node-red-node-google -g \# sun events module sudo npm install
node-red-contrib-sunevents -g \# Json path sudo npm install
node-red-contrib-jsonpath -g \# geofence module, check if localisation
is in zone sudo npm install node-red-node-geofence -g \# geohas, decode
latitude longitude from string sudo npm install node-red-node-geohash -g
\# Foursquare, recommandation on location sudo npm install
node-red-node-foursquare -g \# Ping sudo npm install
node-red-contrib-advanced-ping -g sudo npm install node-red-node-ping -g
\# WOL sudo npm install node-red-node-wol -g \# SNMP sudo npm install
node-red-node-snmp -g \# Weather sudo npm install
node-red-node-forecastio -g sudo npm install
node-red-node-openweathermap -g sudo npm install
node-red-node-weather-underground -g \# General GPIO sudo npm install
node-red-contrib-gpio -g \# Electirc Imp sudo npm install imp-io -g \#
Spark Core sudo npm install spark-io -g \# Arduino/Firmata sudo npm
install firmata -g \# Pushover sudo npm install node-red-node-pushover
-g \# Notify My Android sudo npm install node-red-node-nma -g \#
Pushbullet sudo npm install node-red-node-pushbullet -g \# Prowl sudo
npm install node-red-node-prowl -g \# XMPP sudo npm install
node-red-node-xmpp -g \# IRC sudo npm install node-red-node-irc -g \#
Slack sudo npm install node-red-contrib-slack -g \# Pusher sudo npm
install node-red-node-pusher -g \# Stockage sudo npm install
node-red-node-dropbox -g sudo npm install node-red-node-flickr -g sudo
npm install node-red-node-aws -g sudo npm install node-red-node-box -g
\# Musique sudo npm install node-red-contrib-mpd -g sudo npm install
node-red-contrib-mopidy -g \# Activities sudo npm install
node-red-node-fitbit -g sudo npm install node-red-node-jawboneup -g sudo
npm install node-red-node-strava -g \# KNX/EIBD sudo npm install
node-red-contrib-eibd -g \# OpenZwave sudo npm install
node-red-contrib-openzwave -g \# RFXcom sudo npm install
node-red-contrib-rfxcom -g \# OWFS sudo npm install
node-red-contrib-owfs -g \# Nest sudo npm install node-red-contrib-nest
-g \# Hue sudo npm install node-red-contrib-hue -g \# Spark-Core sudo
npm install node-red-contrib-sparkcore -g \# Wemo sudo npm install
node-red-node-wemo -g \# Zibase sudo npm install node-red-contrib-zibase
-g \# SensorTag sudo npm install node-red-node-sensortag -g \#
Blinkstick sudo npm install node-red-node-blinkstick -g \# Blink1 sudo
npm install node-red-node-blink1 -g \# Tellstick sudo npm install
node-red-contrib-tellstick -g \# PiTFT \#sudo npm install
node-red-contrib-pitft-touch -g \# Pibrella \#sudo npm install
node-red-node-pibrella -g \#sudo apt-get -y install python-rpi.gpio \#
PiBord \#sudo npm install node-red-node-ledborg -g \# Sensors \#sudo npm
install node-red-contrib-bmp085 -g \#sudo npm install
node-red-contrib-ds18b20-sensor -g \#sudo npm install
node-red-contrib-dht-sensor -g \# GPIO \# HummingBoard \#sudo npm
install node-red-node-hbgpio -g \#sudo cp
node\_modules/node-red-node-hbgpio/gpiohb /usr/local/bin/ \#sudo chmod
4755 /usr/lcoal/bin/gpiohb \# Raspberry Pi \#sudo npm install raspi-io
-g \# BeagleBone Black \#sudo npm install beaglebone-io -g \#
Galileo/Edison \#sudo npm install galileo-io -g \# Blend Micro \#sudo
npm install blend-micro-io -g \# LightBlue Bean \#sudo npm install
bean-io -g

