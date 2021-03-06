

Comandos utiles si esta perdido
================


### cambiar nombre de maquina o hostname ###

`hostname mimaquina`

`echo "mimaquina" > /etc/hostname`

`sed -i -r 's#127.0.1.1.*#127.0.1.1   mimaquina#g' /etc/hosts`

### direccion de red y activacion ###

| que comando | ifconfig (solo en ruta de root) | ip (puede correrse como usuario) |
| ----------- | ------------------------------- | -------------------------------- |
| ubicacion   | `/sbin/ifconfig`                | `/bin/ip`                        |
| ver ip (v4) | `ifconfig|grep "inet add"`      | `ip -4 addr|grep inet`           |
| ver activas | `ifconfig|grep "inet add"`      | `ip link ls up|grep mtu`         |
| bajar eth0  | `ifconfig eth0 down`            | `ip link set dev eth0 down`      |
| activar wlan0 | `ifconfig wlan0 up`           | `ip link set dev wlan0 up`       |
| mas MTU eth0 | `ifconfig eth1 mtu 9000 up`    | `ip link set mtu 9800 dev eth0`  |

### detectar que nombre de dispositivo de red puso linux ###

`ls /sys/class/net`

| tipo     | en sysvinit | en systemd | ejemplo |
| -------- | ----------- | ---------- | ------- |
| ethernet | eth         | en         | eth0    |
| serial/slip | slip     | sl         | slip0   |
| inalambr | wlan        | wl         | wlan0   |
| wan inalam | wwan      | ww         | wwan0   |
| punto punto | ppp      | pp         | ppp0    |
| privada  | vpn         | vn         | vpn0    |

**NOTA**: las VPN mas comunes son tecnologia vieja asi como coneciones win, son "ppp" en vez de "vpn".

**NOTA**: en casos wifi (wlan) o seriales (serio) no es tratado como tal y usan "eth"

### configurar un dispositivo de red ###

Redes cableadas:

```
cat > /etc/network/interfaces << EOF
auto lo
iface lo inet loopback
EOF

setup-interfaces -a

echo -e "\n" | setup-dns 8.8.8.8

rc-service networking restart
```

Redes inalambricas de casa:

WIP

### activar los repositorios

```
cat > /etc/apk/repositories << EOF
http://mirror.math.princeton.edu/pub/alpinelinux/v$(cat /etc/alpine-release | cut -d'.' -f1,2)/main
http://mirror.math.princeton.edu/pub/alpinelinux/v$(cat /etc/alpine-release | cut -d'.' -f1,2)/community
http://dl-cdn.alpinelinux.org/alpine/v$(cat /etc/alpine-release | cut -d'.' -f1,2)/main
http://dl-cdn.alpinelinux.org/alpine/v$(cat /etc/alpine-release | cut -d'.' -f1,2)/community
EOF

apk update
```


discos
====================================

### ver identificacion de los discos detectados ###

`/proc/partitions`

| tipo     | nombre | ejemplo  | 
| -------- | ------ | -------- |
| IDE  (*) | hd     | hda,hda1 |
| SATA     | sd     | sda,sda1 |
| NVM      | nvme   | nvme1,nvme1p1 |
| BR/CD/DVD | sr    | sr0      |
| CSD      | mmcblk | mmcblk0,mmcblk0p1 |
| SATA     | sd     | sda,sda1 |

**NOTA** siendo RAID o LVM, el disco es detectado despuea que el RAID o LVM este inicializado.

## Ajustar el syslinux bootloader despeus de instalar en una sola particion

Esto asumiendo que "sda" es el disco duro y "sda1" es la particion unica usada, 
y esta particion "sda1" es donde se instalo previamente montada en "/mnt":

```
sed -s -i -r 's|KERNEL /vmlinuz|KERNEL /boot/vmlinuz|g' /mnt/boot/extlinux.conf
sed -s -i -r 's|INITRD /initramfs|INITRD /boot/initramfs|g' /mnt/boot/extlinux.conf

dd if=/usr/share/syslinux/mbr.bin of=/dev/sda
```



# Vease tambien:

* [../README-md (indice)](../README-md)
* [Informaciones y notas](../informes/)
* Instalarla: [../instalar/README.md](../instalar/README.md)
* Escritorio multimedia: [programas-escritorio-xfce4-uso-general.md](programas-escritorio-xfce4-uso-general.md)
