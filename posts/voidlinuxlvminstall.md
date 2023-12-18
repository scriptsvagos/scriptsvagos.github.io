# Instalar VoidLinux + LVM (UEFI)

Recomiendo usar la iso xfce para ahorrarse tener que copiar y pegar todo, de igual forma pueden hacer lo que quieran xd

entrar a root, la contraseña es: `voidlinux`

Importante Ejecutar: `xbps-install -Su xbps -y`


Ejecutar `lsblk` para ver los dispositivos disponibles, luego ejecutan cfdisk con el dispositivo que eligieron:

    # cfdisk /dev/sda

En mi caso solo crearé 2 particiones: `/dev/sda1` y `/dev/sda2`

Le doy formato solo a la partición que usare de boot/efi

    # mkfs.vfat -F32 /dev/sda1


## LVM:

Creare el volumen fisico en `/dev/sda2`:

    # pvcreate /dev/sda2
    
Creamos el grupo `os`:

    # vgcreate os /dev/sda2

Y dentro crearemos los volumenes logicos, en mi caso 2gb para swap y el restante para root (/)

    # lvcreate -L 2G os -n swap
    # lvcreate -l 100%FREE os -n root

Le doy formato:

    # mkswap /dev/os/swap
    # mkfs.ext4 /dev/os/root



Montamos los volumenes:

    # mount /dev/os/root /mnt
    # mkdir -p /mnt/boot/efi/
    # mount /dev/sda1 /mnt/boot/efi/
    # swapon /dev/os/swap


## Instalación Base:

    # mkdir -p /mnt/var/db/xbps/keys
    # cp /var/db/xbps/keys/* /mnt/var/db/xbps/keys/

    # xbps-install -Sy -R https://repo-default.voidlinux.org/current -r /mnt base-system lvm2 grub-x86_64-efi



## Chroot:

    # xchroot /mnt
    # chown root:root /
    # chmod 755 /

Crean una password para el usuario root:
    
    # passwd root


Crean su usuario y le ponen contraseña, en mi caso:

    # useradd -mG wheel d33vliter
    # passwd d33vliter
    
Escriben `visudo` y descomentan algo que diga `wheel`

Le ponen el hostname a su maquina, en mi caso:

    # echo potato-virtual > /etc/hostname

Agregan idioma al locale, en mi caso:

    # echo "LANG=es_PA.UTF-8" > /etc/locale.conf

Dentro de `/etc/default/libc-locales` descomentan lo que tenga su idioma, en mi caso: `LANG=es_PA.UTF-8`, tambien la ISO

Seguido ejecutamos esto para configurar todos los locale:
    
    # xbps-reconfigure -f glibc-locales



## FSTAB:

dentro del `/etc/fstab`, agrego esto:

    tmpfs	/tmp	tmpfs	defaults,nosuid,nodev	0       0
    /dev/os/root	/	ext4	defaults	0       0
    /dev/os/swap  swap	swap    defaults	0       0
    /dev/sda1	/boot/efi	vfat	defaults	0	0


Instalo el grub:

    # grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id="Void"

Ejecutamos esto para asegúrarnos que se generen los initramfs:

    # xbps-reconfigure -fa


Salimos y reiniciamos: 
    
    # exit
    # umount -R /mnt
    # shutdown -r now


Listo!, ya tienen VoidLinux instalado con LVM :D
<p>&nbsp;</p>
<p>&nbsp;</p>


## Adicional(lo pongo aqui para no olvidarlo):


extender espacio:

Selecciono el dispositivo que agregare al grupo:

    vgextend os /dev/sdc
    
 Extiendo el espacio del volumen que quiera:

    lvextend -l +100%FREE /dev/os/root
    
 Aplico cambios en el filesystem

    resize2fs /dev/os/root
