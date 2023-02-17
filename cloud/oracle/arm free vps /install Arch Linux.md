credits: https://gist.github.com/encombhat/e49301b673bd39862251a16c9bcd6ba5

in Oracle's distro: curl -sSL [NYI] | sh

````sh
cd /tmp
wget https://dl-cdn.alpinelinux.org/alpine/v3.13/releases/aarch64/alpine-virt-3.13.5-aarch64.iso
dd if=alpine-virt-3.13.5-aarch64.iso of=/dev/sda
sync
reboot
````

in Alpine w/ Console access:
````sh
echo "auto eth0" > /etc/network/interfaces
echo "iface eth0 inet dhcp" >> /etc/network/interfaces
ifup eth0
````
once you have access to internet: curl -sSL [NYI] | sh
````sh
echo "auto eth0" > /etc/network/interfaces
echo "iface eth0 inet dhcp" >> /etc/network/interfaces
ifup eth0
mkdir /media/setup
cp -a /media/sda/* /media/setup
mkdir /lib/setup
cp -a /.modloop/* /lib/setup
/etc/init.d/modloop stop
umount /dev/sda
mv /media/setup/* /media/sda/
mv /lib/setup/* /.modloop/
setup-apkrepos
sed -i "/community/s/^#//g" /etc/apk/repositories
apk update
apk add dosfstools e2fsprogs libarchive-tools pacman arch-install-scripts
````
use any partition program to obtain something like this:
````
Disk /dev/sda:
Disklabel type: gpt

Device       Start       End   Sectors   Size Type
/dev/sda1  1050624 419430366 418379743 199.5G Linux filesystem
/dev/sda15    2048   1050623   1048576   512M EFI System
````

mount and install: 
````sh 
mkfs.vfat /dev/sda15
mkfs.ext4 /dev/sda1
mount /dev/sda1 /mnt
mkdir -p /mnt/boot
mount /dev/sda15 /mnt/boot
cd /mnt
wget http://os.archlinuxarm.org/os/ArchLinuxARM-aarch64-latest.tar.gz
bsdtar -xpf /mnt/ArchLinuxARM-aarch64-latest.tar.gz -C /mnt
genfstab -U /mnt >> /mnt/etc/fstab
arch-chroot /mnt/
pacman-key --init
pacman-key --populate archlinuxarm
pacman -Syu efibootmgr
bootctl install
echo "title   Arch Linux
linux   /Image
initrd  /initramfs-linux.img
options root=PARTUUID=$(blkid -s PARTUUID -o value /dev/sda1) rw" > /boot/loader/entries/arch.conf
````