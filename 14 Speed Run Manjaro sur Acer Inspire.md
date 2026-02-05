## Speed Run Manjaro sur Acer Inspire

Réalisé sur `es1-732-c2mr`

System BIOS Version `V1.18`

## Objectif

Un ordinateur Acer Inspire bootant sur Manjaro

## Préparatifs

Un point d'accès WIFI connecté à internet (OPTIONAL mais préférable)

Une clé USB bootable avec Manjaro installé dessus connectée sur le port le plus rapide

(TOConfirm)
Configuration de la carte mère via le menu constructeur

    Mise sous tension de la machine
    Laisser le doigt appuyé sur la touche `<F2>`

À l'ouverture du menu de configuration de la carte mère:

Obligation d'avoir un mot de passe Supervisor pour pouvoir désactiver le Secure Boot UEFI

    `Security > Set Supervisor Password`

Désactiver Secure Boot

    `Boot > Secure Boot` mis sur `Disabled`

Changer l'ordre du boot pour booter sur la clé USB en premier

    `Boot > Boot priority order` sur `USB HDD`
    # Faites en sorte de booter sur votre média d'installation en premier 
    # changer l'ordre avec les touches `<F5>` et `<F6>`

Sauvegarder vos modifcations et relancer la machine

    `Exit > Exit Saving Changes`
 
## Premier boot

Boot live avec drivers open source (Les drivers Intel Graphics sont normalement open source)

Connection au Wifi (OPTIONAL)

Lancer l'install

Configurer selon vos convenaces et Confirmer l'install

À l'étape de l'installation du bootloader (environ 90%) attendre que l'ordinateur se freeze

Forcer l'extinction de la machine en laissant le bouton d'alimentation appuyé

La machine est partitionnée et l'installation logicielle est complète, reste à finir la configuration du boot

## Deuxième boot

Boot live avec drivers open source

Ouverture d'un terminal

    # Snippet adapté si la commande
    # sudo `lsblk | grep sda | cut -d " " -f 1` à la sortie suivante:
    # sda
    # ├── sda1 <- ESP
    # └── sda2 <- Manjaro
    su
    mount -o subvol=@ /dev/sda2 /mnt    # Si BTRFS
    mount /dev/sda2 /mnt                # Si ext4, ext3 ...

    mount /dev/sda1 /mnt/boot/efi
    for i in /dev /dev/pts /proc /sys ; do mount -B $i /mnt$i ; done
    chroot /mnt
    grub-install --no-nvram
    mkdir -p /boot/efi/EFI/BOOT
    cp -f /boot/efi/EFI/manjaro/grubx64.efi /boot/efi/EFI/BOOT/BOOTx64.efi
    update-grub
    exit
    exit
    halt    # Pour avoir l'occasion d'enlever la clé USB machine éteinte

La machine est prète pour pouvoir démarrer sans aide externe.

    Enlever la clé et relancer votre machine.
    Le troisième boot devrait bien se dérouler maintenant.
