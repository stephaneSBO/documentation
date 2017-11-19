Objectif
========

Voici, au cas où, une procédure de bout en bout pour installer une clé
WiFi de type "Edimax ew-7811n" sur une Jeedom Mini, et en considérant
une authentification wifi de type "WPA 2 + AES".

Attention, cette procédure ne marche telle quelle qu’uniquement dans le
strict cadre de l’environnement indiqué précédemment (même modèle, même
box Jeedom, même authentification WiFi) . Bien entendu cela peut
fonctionner également pour d’autres composants (ou pas) mais sans aucune
garantie.

> **Important**
>
> Attention cette procédure n’est surtout pas à faire sur mini+ sous
> peine de perdre l’accès à celle-ci et de devoir tout réinstaller.

> **Important**
>
> Cette doc est à appliquer à vos risques et périls en cas de soucis
> l'équipe Jeedom ne pourra en aucun cas être tenue responsable.

Vérification que la clé WiFi est bien reconnue
==============================================

Il suffit de saisir, en SSH, la commande suivante :

    sudo lsusb | grep Edimax

Si la clé est bien reconnue, le message ci-dessous doit alors s’afficher
:

    Bus 001 Device 004: ID 7392:7811 Edimax Technology Co., Ltd EW-7811Un 802.11n Wireless Adapter [Realtek RTL8188CUS]

Les identifiants du Bus et du Device seront peut-être différents chez
vous, selon le port USB sur lequel vous avez branché votre clé.

Vérification du chargement du driver
====================================

La clé WiFi Edimax a l’avantage d’avoir un pilote déjà intégré dans
votre Mini et il n’y a qu'à le vérifier en saisissant la commande
suivante en SSH :

    sudo lsmod | grep 8192cu

Si la commande renvoie une valeur, c’est que tout est OK. Par exemple
chez moi, j’obtiens en retour cela :

    8192cu                550797  0

==Édition du fichier "/etc/network/interfaces"

Il vous faut d’abord éditer le fichier "/etc/network/interfaces" avec la
commande suivante :

    sudo nano /etc/network/interfaces

Voici le contenu du fichier opérationnel chez moi :

    auto lo
    iface lo inet loopback
    iface eth0 inet dhcp
    #wlan
    #=============
    auto wlan0
    allow-hotplug wlan0
    iface wlan0 inet dhcp
    pre-up wpa_supplicant -Dwext -i wlan0 -c /etc/wpa_supplicant.conf -B
    iface default inet dhcp

Èdition du fichier "/etc/wpa\_supplicant.conf"
==============================================

Il ne reste maintenant plus qu'à insérer vos paramètres WiFi (nom de
votre SSID et clé WPA). Cela se fait en éditant le fichier
/etc/wpa\_supplicant.conf avec la commande :

    sudo nano /etc/wpa_supplicant.conf

Voici mon fichier opérationnel chez moi :

    ctrl_interface=/var/run/wpa_supplicant
    update_config=1
    eapol_version=1
    ap_scan=2
    network={
            ssid="NOM_DE_TON_RESEAU_SSID"
            scan_ssid=1
            mode=0
            proto=WPA2
            pairwise=CCMP
            group=CCMP
            key_mgmt=WPA-PSK
            # choose one of the following
            psk="TA_CLE_WIFI"
     }
     ----

     Attention à remplacer les paramètres indiqués ci-dessous par les vôtres :

    * NOM_DE_TON_RESEAU_SSID par le nom de votre propre réseau, en gardant bien les guillemets (" ")
    * TA_CLE_WIFI par le nom de votre propre réseau, en gardant bien les guillemets (" ")

    J'attire également votre attention que votre clé WiFi apparaîtra en clair dans le fichier. Si vous souhaitez plus de sécurité, vous pouvez encoder au préalable votre clé via la commande "sudo wpa_passphrase" puis insérer votre clé encodée (sans mettre les guillemets dans ce cas-là).

    == Activation du WiFi

    Une fois que les fichiers sont renseignés, il ne reste plus qu'à démarrer la connexion WiFi, en tapant la commande suivante :

sudo ifup wlan0

    Normalement, votre WiFi devrait être opérationnel sur votre Mini.
