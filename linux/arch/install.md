I'm writing this because arch wiki ain't deep enough.

in modern archiso you can just use arch-install

### 0 EFI Only

this document will talk about the installation on modern computer

to check if you booted correctly with modern UEFI
````
ls /sys/firmware/efi/efivars
````
or 
````
efivars -l
````

if the output is not present you booted as legacy

### 1 LOCAL or REMOTE install:
#### 1.1 LOCAL

to use your local keyboard properly

````
loadkeys us-acentos
````
#### 1.2 REMOTE
to remote access via ssh you need to set a password and launch the ssh deamon
````


````




# Install arch, the raw way, DRAFT, not verbose

COMMAND

|  | ALTERNATIVES | DESCRIPTION |
| --- | --- | --- |
| ls /sys/firmware/efi/efivars | efivars -l | verify UEFI or legacy boot mode |
| ping simpolab.com |  | verify internet connection |
| iwctl |  | connect to wifi |
| loadkeys it | us-acentos | systemctl start sshd && passwd | set keymap for local install or
start sshd for remote install |
| timedatectl set-ntp true |  | sync time |
| fdisk -l | lsblk -f | show disks |
| fdisk    /dev/… | parted /dev/… or whatever other partition tool | part your disks |
| mkfs.ext4 /dev/…
mkfs.fat -F 32 /dev/…
mkswap /dev/… | btrfs, xfs or any other you prefer
do not format EFI part if windows is already installed
do not swap on ssd, please. | format your partitions |
| swapon /dev/…
mount /dev/… /mnt
mount /dev/… /mnt/boot |  | mount your partitions |
| systemctl start reflector |  | find best remotes for pacman |
| pacstrap /mnt base |  | install base on /mnt |
| genfstab -U /mnt >> /mnt/etc/fstab |  | save actual mount status of /mnt into the file fstab, manual edit if needed |
| arch-chroot /mnt |  | execute as installed system |
|  | ln -s /usr/share/zoneinfo/Europe/Rome /etc/localtime | set localtime |
| pacman-key --init
pacman-key --populate |  | if needed |
| editor /etc/pacman.conf |  | if needed add repos : multilib, chaotic aur and all what you need   (colors ILoveCandy Parallels) |
| visudo |  | enable sudo and wheel groups |
| groupadd sudo
useradd -m -G sudo,wheel x | #useradd -d /home/zyaen zyaen  |
#mkdir /home/zyaen
#chown zyaen /home/zyaen/
#usermod –aG wheel nomeutente | create group sudo
create user x with groups sudo and wheel |
| passwd x |  | change password for user x |
| passwd |  | change password for user root, if needed |
| localectl | #vi /etc/locale.conf LANG=en_US.UTF-8
#vi /etc/vconsole.conf KEYMAP=us | change locale |
| hostnamectl hostname y | #vi /etc/hostname y
#vi /etc/hosts add y | change hostname |
| pacman -Sy ucode 
pacman -S linux-firmware 
pacman -S linux linux-headers  | (amd-ucode intel-ucode intel-ucode-clear)
(linux-firmware-qcom linux-firmware)
(linux-zen linux-xanmod linux-clear) | install the right ucode for your system
same for linux-firmware
and kernel |
| bootctl --path=/boot/ install |  | install systemd-boot |
| edit /boot/loader/entries/name.conf |  | create a file this:   launch a “ls boot” for show the right files
title	OSname
linux	/vmlinuz-linux
initrd	/x-ucode.img
initrd	/initramfs-linux.img
options root=PARTUUID=(blkid -s PARTUUID -o value /dev/sdXN) rw |
| systemctl enable systemd-networkd
systemctl enable systemd-resolved
systemctl enable systemd-timesyncd
systemctl enable NetworkManager
systemctl enable sshd
systemctl enable gdm
systemctl enable reflector.timer
ecc |  | enable all the needed things |
| exit |  | exit chroot |
| cp -r /etc/systemd/network /mnt/etc/systemd |  | copy network settings for iso to installation |
| reboot |  |  |



# misc not sorted

paru -Syu man-pages man db reflector

mkinitcpio -p linux | -P      regenerate kernel

cp  /etc/xdg/reflector/reflector.conf /mnt/etc/xdg/reflector/

vi /etc/locale.gen it_IT
locale-gen
localectl set-keymap us-acentos