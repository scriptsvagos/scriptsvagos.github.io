# Eliminar Tearing Intel Linux

Solucion para evitar el screen tearing en distros GNU/Linux(en caso te pase)

Por lo general este fix es para entornos de escritorio como XFCE(en la version 4.14 lo solucionan)



`sudo mkdir /etc/X11/xorg.conf.d`

`sudo nano /etc/X11/xorg.conf.d/20-intel.conf`

dentro de 20-intel.conf poner lo siguente:

    Section "Device"
        Identifier "Intel Graphics"
        Driver "intel"
        Option "TearFree" "true"
    EndSection
