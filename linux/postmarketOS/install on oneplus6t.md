credits && resources: https://wiki.postmarketos.org/wiki/

### Install
````
pmbootstrap init
pmbootstrap install --split
pmbootstrap export
pmbootstrap shutdown
````
navigate to /tmp/postmarketOS-export and flash this files with fastboot
| file | where to flash |
|--|--|
| boot.img | boot |
| initramfs  initramfs-extra |  |
 oneplus-fajita-boot.img | vendor |
 oneplus-fajita-root.img | userdata 
| vmlinuz |  |

````fastboot
fastboot erase dtbo
````
### DONE
inside postmarketOS: 
````
sudo resize2fs /dev/sda17
```` 
### CONNECT VIA USB
````
ip link set dev usb0 address 12:12:12:12:12:12
dhclient -v usb0
ip link set usb0 up
````
### DNS
````
echo nameserver 1.1.1.1 > /etc/resolv.conf
````
### INTERNET VIA USB
````
ip route add default via 172.16.42.2
````
#### persistent INTERNET VIA USB
````
echo 'ip route add default via 172.16.42.2' > /etc/local.d/usb_internet.start
chmod +x /etc/local.d/usb_internet.start
rc-update add local
````
#### on HOST
````
sysctl net.ipv4.ip_forward=1
iptables -A FORWARD -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
iptables -A FORWARD -s 172.16.42.0/24 -j ACCEPT
iptables -A POSTROUTING -t nat -j MASQUERADE -s 172.16.42.0/24
iptables-save
````