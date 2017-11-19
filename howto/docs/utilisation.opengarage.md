OpenGarage est un objet de type DIY mais aussi disponible monté sur
commande et qui sert dans le garage.

Il propose l’activation d’un relais (pour ouvrir le garage) et un
capteur de distance pour vérifier la présence de la voiture.

<http://opengarage.io/>

Lecture des états du OpenGarage
===============================

Afin de récupérer l'état du relais et du capteur de distance, l’url à
utiliser est :

    http://addropengarage/jc

Le résultat est un json. Il faut donc utiliser un equipement de type
Script et une commande info de type json

Pour l'état du relais le nom de la propriété du json : door

Pour le capteur de distance : dist

Action sur le OpenGarage
========================

L’adresse pour l’activation du relais est :

    http://addropengarage/cc?dkey=xxxx&click=1

dkey est la clef de l’api, par défaut c’est opendoor

Plus d’infos
============

La documentation complète de l’API est disponible sur github :

<https://github.com/OpenGarage/OpenGarage-Firmware/tree/master/docs>

