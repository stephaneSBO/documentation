Objectif
========

Vous trouverez ici la documentation pour installer Jeedom sur un
raspberry PI 3 **sans carte SD.**

Le PI3 offre en effet la possibilité de booter directement sur un
périphérique USB et donc de vous affranchir de la carte SD parfois
génératrice de problémes (corruption).

**La procédure d’installation est strictement identique à celle sur une
SD, mais il faudra s’assurer de posséder un Firmware à jour.**

Pour cela ouvrez une connexion SSH. (si vous ne savez pas comment faire,
regarder installation sur SD :
[Ici](https://jeedom.github.io/documentation/installation/fr_FR/index.html)
)

    vcgencmd otp_dump | grep 17:

Vous devez obtenir en retour :

    17:3020000a

Si c’est le cas, votre PI3 est correctement configuré pour booter sur
l’USB. Si il ne trouve rien, il démarrera normalement sur une carte SD.

Si le retour est différent, vous devez simplement effectuer une mise à
jour.

    sudo apt-get update; sudo apt-get install rpi-update

puis

    sudo rpi-update

Puis reboot du PI3

    sudo reboot

> **Important**
>
> Pour éviter les problèmes de puissance, optez pour un disque SSD MSATA
> à faible consommation.

> **Tip**
>
> Vous pouvez désormais installer Jeedom en suivant exactement la même
> procédure qu’avec une carte SD.
> [Ici](https://jeedom.github.io/documentation/installation/fr_FR/index.html)

**Il faut ensuite prendre en compte les remarques suivantes :**

> **Warning**
>
> Les modifications suivantes sont le fruit de problémes rencontrés par
> les utilisateurs. Vous devez les adapter à votre cas. Le support
> Jeedom n’intervient pas pour des problémes liés à votre configuration.

**1/ Si vous rencontrez des problèmes de swap, il faut le modifier.**

**Augmenter sa taille** :

    sudo nano /etc/dphys-swapfile

    CONF_SWAPSIZE=100

Changer la valeur de CONF\_SWAPSIZE pour 1024 par exemple, puis reboot.

**Changer la valeur d’appel au swap.**

Par défaut, le système appelle le swap losqu’il reste moins de 40% de
Ram.

    sudo nano /etc/sysctl.conf

Ajouter la ligne :

    vm.swappiness = 10

Pour demander au Pi de n’appeler le swap que lorqu’il lui reste 10% de
mémoire disponible (soit 100 MO de Ram disponible). Puis reboot.

**Désactiver le bluetooth intégré car incompatible avec la carte GPIO
zwave.me**

    sudo nano /boot/config.txt

ajouter la ligne

    dtoverlay=pi3-disable-bt

Faire un arrêt propre

    sudo halt

Débrancher rebrancher (pas de sudo reboot !).

