Nous allons voir ici comment changer votre mot de passe Jeedom si vous
l’avez oublié directement en modifiant celui-ci en base de données.

La première chose est de se connecter en SSH à Jeedom (avec un logiciel
type kitty ou putty). Vous trouverez les identifiants SSH
[ici](https://jeedom.fr/doc/documentation/installation/fr_FR/doc-installation.html).

Une fois connecté il vous faut récuperer les identifiants de la base de
données :

``` {.bash}
cat /var/www/html/core/config/common.config.php
```

Ici vous trouverez le mot de passe pour accéder à la base de données
jeedom, il faut ensuite faire :

``` {.bash}
mysql -ujeedom -p
```

Là il vous demande le mot de passe récupéré plus haut. Tapez ensuite :

``` {.bash}
use jeedom;
REPLACE INTO user SET `login`='admin',password='c7ad44cbad762a5da0a452f9e854fdc1e0e7a52a38015f23f3eab1d80b931dd472634dfac71cd34ebc35d16ab7fb8a90c81f975113d6c7538dc69dd8de9077ec',profils='admin', enable='1';
```

Voilà vous pouvez vous connecter à nouveau à votre jeedom avec les
identifiants admin/admin ce qui vous permettra de modifier le mot de
passe des autres comptes.

