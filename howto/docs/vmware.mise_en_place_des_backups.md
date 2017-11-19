Il est important d’avoir des backups de ses VMs et c’est un point à ne
surtout pas négliger, sans même parler de pannes matérielles vous pouvez
un jour avoir besoin de revenir à un backup suite à une mauvaise
manipulation ou un problème suite à une mise à jour. Attention ici on
parle d’image complète des VMs, ce n’est pas juste un backup applicatif,
il aura donc une taille assez importante.

Une des contrainte pour faire un backup sous VMware est d’avoir
absolument 2 datastores. Pour cela vous avez plusieurs choix :

-   2 disques durs/SSD avec un datastore sur chaque

-   un NAS (type Synology) qui partage un montage NFS. Dans ce cas il
    faut ajouter un système de fichier réseaux à VMware pour qu’il voit
    celui-ci comme un datastore

Pour ce tuto je vais utiliser l’interface Web de l’ESXi qui est
disponible soit par l’installation d’un vib soit à partir de la version
6.0 update 2. Pour rappel, pour accéder à cette interface il suffit
d’aller sur IP\_ESXI/ui

> **Note**
>
> Pour ce tuto je vais utiliser l’interface Web de l’ESXi qui est
> disponible soit par l’installation d’un vib soit à partir de le
> version 6.0 update 2. Pour rappels pour acceder à cette interface il
> suffit d’aller sur IP\_ESXI/ui

Installation de ghettoVCB
=========================

Il faut récupérer ce
[script](https://raw.githubusercontent.com/lamw/ghettoVCB/master/ghettoVCB.sh)
et le transférer vers l’ESXi (sur le même datastore que celui qui va
accueillir les backups par exemple).

> **Note**
>
> Dans la suite de ce tuto je considère que vous avez mis le script
> ghettoVCB.sh dans /vmfs/volumes/Backup/ghettoVCB.sh. A vous d’adapter
> en fonction de votre configuration les commandes/scripts fournis.

Connexion en ssh
================

Il va falloir vous connecter en SSH sur l’ESXi, pour ce faire il faut à
partir de l’interface

![vmware.backup](images/vmware.backup.PNG)

Ensuite avec putty ou kitty connectez-vous dessus en mettant l’IP de
votre ESXi et en utilisant vos identifiants de celui-ci

Création du fichier de configuration
====================================

> **Note**
>
> Pour toute la suite de ce tuto je considère que votre datastore de
> backup a pour chemin /vmfs/volumes/Backup, attention à bien changer si
> ce n’est pas le cas chez vous

Sur le datastore de backup il faut créer un fichier ghettoVCB.conf qui
contient :

    VM_BACKUP_VOLUME=/vmfs/volumes/Backup/
    DISK_BACKUP_FORMAT=thin
    VM_BACKUP_ROTATION_COUNT=2
    POWER_VM_DOWN_BEFORE_BACKUP=0
    ENABLE_HARD_POWER_OFF=0
    ITER_TO_WAIT_SHUTDOWN=3
    POWER_DOWN_TIMEOUT=5
    ENABLE_COMPRESSION=0
    VM_SNAPSHOT_MEMORY=0
    VM_SNAPSHOT_QUIESCE=0
    ALLOW_VMS_WITH_SNAPSHOTS_TO_BE_BACKEDUP=0
    ENABLE_NON_PERSISTENT_NFS=0
    UNMOUNT_NFS=0
    NFS_SERVER=172.30.0.195
    NFS_MOUNT=/nfsshare
    NFS_LOCAL_NAME=nfs_storage_backup
    NFS_VM_BACKUP_DIR=mybackups
    SNAPSHOT_TIMEOUT=15
    EMAIL_LOG=0
    EMAIL_SERVER=auroa.primp-industries.com
    EMAIL_SERVER_PORT=25
    EMAIL_DELAY_INTERVAL=1
    EMAIL_TO=auroa@primp-industries.com
    EMAIL_FROM=root@ghettoVCB
    WORKDIR_DEBUG=0
    VM_SHUTDOWN_ORDER=
    VM_STARTUP_ORDER=

Les paramètres que vous devez adapter sont :

-   **VM\_BACKUP\_VOLUME** ⇒ emplacement de votre datastore de backup

-   **VM\_BACKUP\_ROTATION\_COUNT** ⇒ nombre de backup par VM à garder

> **Note**
>
> Vous pouvez consulter
> [ici](https://communities.vmware.com/docs/DOC-8760) la documentation
> complète de ghettoVCB avec une description de chaque paramètre

> **Important**
>
> Attention à bien mettre le / final pour le paramètre
> VM\_BACKUP\_VOLUME sinon le script sera en erreur

Test de backup
==============

Nous allons ici lancer un premier backup initial de toutes les VMs pour
voir si tout est ok. Par la suite nous le planifierons en automatique.
Retourner sur l’ESXi en SSH (reconnectez-vous si nécessaire) et faites :

    /vmfs/volumes/Backup/ghettoVCB.sh -a -g /vmfs/volumes/Backup/ghettoVCB.conf

Cela va lancer un backup de toute vos VMs (et peut donc prendre pas mal
de temps). A la fin vous devriez avoir sur votre datastore de backup un
dossier par VM et dans chaque dossier de VMs un sous-dossier par date
contenant 4 fichiers :

![vmware.backup2](images/vmware.backup2.PNG)

-   \*-flat.vmdk ⇒ le disque virtuel de votre machine

-   \*.vmdk ⇒ le descripteur du disque

-   \*.vmx ⇒ le fichier contenant le configuration de votre machine

-   STATUS.ok ⇒ indique que le backup est bien ok

Voici d’autre possibilité pour la ligne de commande :

-   Simulation de backup :

<!-- -->

    /vmfs/volumes/Backup/ghettoVCB.sh -d dryrun -a -g /vmfs/volumes/Backup/ghettoVCB.conf

-   Lancement en mode debug :

<!-- -->

    /vmfs/volumes/Backup/ghettoVCB.sh -d debug -a -g /vmfs/volumes/Backup/ghettoVCB.conf

-   Backup seulement la VM "toto"

<!-- -->

    /vmfs/volumes/Backup/ghettoVCB.sh -m toto -a -g /vmfs/volumes/Backup/ghettoVCB.conf

Lancement automatique des backups
=================================

Il faut ajouter la ligne de commande à la crontab mais sous VMware la
crontab est un peu spéciale et surtout écrasée à chaque démarrage. Pour
éviter cela il faut donc ajouter un petit script qui mettra à jour la
crontab au boot (ne vous inquiétez pas c’est assez simple et rapide), en
SSH sur l’ESXi faire :

    vi /etc/rc.local.d/local.sh

Et avant le "exit 0" ajouter les lignes suivantes :

    /bin/kill $(cat /var/run/crond.pid)
    /bin/echo "0 0 1 * * /vmfs/volumes/Backup/ghettoVCB.sh -a -g /vmfs/volumes/Backup/ghettoVCB.conf >/dev/null 2>&1" >> /var/spool/cron/crontabs/root
    /usr/lib/vmware/busybox/bin/busybox crond

> **Note**
>
> Ici je demande un backup tous les 1er du mois, vous pouvez changer
> cela en modifiant : 0 0 1 \* \*

> **Note**
>
> Ici je fais un backup de toute les VMs, vous pouvez adapter cela en
> remplaçant le -a par -m ma\_vm, attention si vous voulez mettre
> plusieurs VMs il faut dupliquer la ligne "/bin/echo "0 0 1 \* \*
> /vmfs/volumes/Backup/ghettoVCB.sh -a -g
> /vmfs/volumes/Backup/ghettoVCB.conf \>/dev/null 2\>&1" \>\>
> /var/spool/cron/crontabs/root" et en mettre une par VM à backuper

> **Important**
>
> N’oubliez pas d’adapter le chemin vers le fichier de configuration de
> ghettoVCB en fonction de votre configuration :
> /vmfs/volumes/Backup/ghettoVCB.conf

Dernière étape: il faut redémarrer votre ESXi pour que le cron soit pris
en compte, vous pouvez voir le résultat en faisant (toujours en SSH) :

    cat /var/spool/cron/crontabs/root

Ici vous devez avoir une ligne :

    0 0 1 * * /vmfs/volumes/Backup/ghettoVCB.sh -a -g /vmfs/volumes/Backup/ghettoVCB.conf >/dev/null 2>&1
