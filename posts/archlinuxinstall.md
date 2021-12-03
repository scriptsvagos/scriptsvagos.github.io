# Guia de instalación Arch Linux

### **Definir la distribución del teclado en el entorno live:**

    # ls /usr/share/kbd/keymaps/**/*.map.gz

En mi caso: 

    # loadkeys us
<p>&nbsp;</p>


### **Verificar la modalidad de arranque**

    # ls /sys/firmware/efi/efivars
<p>&nbsp;</p>


### **Verificar conexion de internet**

    # ping archlinux.org
<p>&nbsp;</p>


### **Actualizar el reloj del sistema**

    # timedatectl set-ntp true
<p>&nbsp;</p>


### **Particionar el disco**

para ver las particiones es `fdisk -l` o `lsblk`.

Una vez identificado el dispositivo haremos la particion con:

    # cfdisk /dev/sdX

En mi caso quedaria asi:

    /dev/sda1 512MB BOOT

    /dev/sda2 248G ROOT

    /dev/sda3 2G SWAP

Vamos a darle formato:

/dev/sda1(boot UEFI): 

    # mkfs.fat -F 32 /dev/sda1

/dev/sda2(/): 

    # mkfs.ext4 /dev/sda2

/dev/sda3(SWAP):

    # mkswap /dev/sda3
<p>&nbsp;</p>


### **Montar particiones**

    # swapon /dev/sda3

    # mount /dev/sda2 /mnt

    # mkdir /mnt/boot

    # mount /dev/sda1 /mnt/boot
<p>&nbsp;</p>


### **Instalar paquetes esenciales**

    # pacstrap /mnt base base-devel linux linux-firmware nano sudo networkmanager
<p>&nbsp;</p>


### **Configuración del sistema**

Fstab:

    # genfstab -U /mnt >> /mnt/etc/fstab

Chroot:

    # arch-chroot /mnt
<p>&nbsp;</p>


### **Zona horaria**

Pueden buscarlo con `ls /usr/share/zoneinfo/`, en mi caso quedaria asi:

    # ln -sf /usr/share/zoneinfo/America/Panama /etc/localtime

    # hwclock --systohc
<p>&nbsp;</p>


### **Idioma del sistema**

Editar `/etc/locale.gen` y descomentar el locale necesario, en mi caso seria es_PA.UTF-8 UTF-8.

Genere los locales ejecutando la orden: 

    # locale-gen

Agrega tu locale en `/etc/locale.conf`:

    LANG=es_PA.UTF-8


Defina la distribución de teclado en `/etc/vconsole.conf`:

    KEYMAP=us
<p>&nbsp;</p>


### **Configurar la red**

Cree el archivo `/etc/hostname`:

    potatin-pc



Considere añadir una entrada similar `/etc/hosts`:

    127.0.0.1	localhost
    ::1	        localhost
<p>&nbsp;</p>


### **Usuarios**

Si queremos agregarle contraseña al usuario root seria:

    passwd (luego les pedira la contraseña, que la repitan, y listo)

En mi caso utilizare un usuario nuevo que tenga esos privilegios, entonces seria lo siguiente:

    nano /etc/sudoers

dentro del archivo sudoers descomentaremos el parámetro     `#%wheel ALL=(ALL) ALL`, quedando así:

    %wheel ALL=(ALL) ALL

    
guardamos y cerramos del archivo.

Ahora crearemos el nuevo usuario con privilegios administrador de la siguiente manera:

    useradd -mG wheel tunombredeusuario
    passwd tunombredeusuario
<p>&nbsp;</p>


### **BootLoader**

Instalar Grub:
    
    pacman -S grub os-prober efibootmgr


Instalar grub en la particion uefi /boot:

    grub-install grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=grub --recheck

por ultimo:

    grub-mkconfig -o /boot/grub/grub.cfg



### **Post Instalación**

Instalar xorg:

    # pacman -S xorg

Xfce4:

    # pacman -S xfce4 xfce4-goodies


LightDM:

    # pacman -S lightdm lightdm-gtk-greeter
