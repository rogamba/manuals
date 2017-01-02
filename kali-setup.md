# Configuraciones Kali Linux live usb

- Descargar Kali Linux
- Descargar Mac USB Loader
- Formatear el USB y hacer tres particiones (mac disk utility)
  - MS-DOS fat con 6 gb para kali linux
  - MS-DOS fat con 6 gb para la persistencia del os
  - La que ya estaba por defecto

# Persistencia
* Cuando se termine de hacer la instalacion del usb, editar archivos de configuacion
* Despues del hostname agregar "persistence" para actvar la opcion de persistencia
* Iniciar Kali linux (vol down surface / alt mac / f12,f2,backspace others)
* Abrir el programa Gparted
* Umount y cambar el format de la particion de la persistencia
* Ejecutar los comandos:   
```shell
$ fdisk -l (ver las particiones y memorias)
$ e2label /dev/sdb<x> persistence (ver el nombre de la particion - sdb1,sdb2,sdb3,...)
$ mkdir -p /mnt/my_usb
$ mount /dev/sdb3<x> /mnt/my_usb
$ echo "/ union" > /mnt/my_usb/persistence.conf 
$ umount /dev/sdb<x>
```

# Creacion de usuario no root
Crear y agregar el nuevo usuario al grupo de sudoers   
```shell
$ useradd -m <username>
$ chsh -s /bin/bash <username>
$ usermod -a -G sudo <username>
```

# Activar driver de sonido
```shell
$ gedit /etc/pulse/daemon.conf 
$ systemctl --user enable pulseaudio
$ systemctl --user start pulseaudio
```

# Wifi
```shell
apt-get update
apt-get install linux-image-$(uname -r|sed 's,[^-]-[^-]-,,') linux-headers-$(uname -r|sed 's,[^-]-[^-]-,,')

wget http://http.kali.org/kali/pool/main/l/linux/linux-kbuild-4.6_4.6.4-1kali1_amd64.deb
wget http://http.kali.org/kali/pool/main/l/linux/linux-headers-4.6.0-kali1-common_4.6.4-1kali1_amd64.deb
wget http://http.kali.org/kali/pool/main/l/linux/linux-headers-4.6.0-kali1-amd64_4.6.4-1kali1_amd64.deb

dpkg -i linux-kbuild-4.6_4.6.4-1kali1_amd64.deb
dpkg -i linux-headers-4.6.0-kali1-common_4.6.4-1kali1_amd64.deb
dpkg -i linux-headers-4.6.0-kali1-amd64_4.6.4-1kali1_amd64.deb

apt-get install broadcom-sta-dkms

modprobe -r b44 b43 b43legacy ssb brcmsmac bcma
modprobe wl
```


# Wireshark, ejecutarlo como non-root user
$ setcap 'CAP_NET_RAW+eip CAP_NET_ADMIN+eip' /usr/sbin/dumpcap

