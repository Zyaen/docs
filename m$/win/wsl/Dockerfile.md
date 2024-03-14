### How to docker in WSL, without docker desktop that sucks
install docker

create a command to script to start it

cat /usr/local/bin/prepare-hosts
```
#!/bin/sh
export WINIP=$(cat /etc/resolv.conf | grep 'nameserver' | cut -f 2 -d ' ')
export WSLIP=$(ip addr show eth0 | grep 'inet ' | cut -f 6 -d ' ' | cut -f 1 -d '/')
echo $WINIP
echo $WSLIP
rm /tmp/hosts
if grep "wsl2" /mnt/c/Windows/System32/drivers/etc/hosts
then
    cp /mnt/c/Windows/System32/drivers/etc/hosts /tmp/hosts
    sed -i "s/.*wsl2/$WSLIP  wsl2/" /tmp/hosts
    cat /tmp/hosts > /mnt/c/Windows/System32/drivers/etc/hosts
else
    echo "$WSLIP  wsl2\n" >> /mnt/c/Windows/System32/drivers/etc/hosts
fi
rm /tmp/hosts
if grep "winhost" /mnt/c/Windows/System32/drivers/etc/hosts
then
    cp /mnt/c/Windows/System32/drivers/etc/hosts /tmp/hosts
    sed -i "s/.*winhost/$WINIP  winhost/" /tmp/hosts
    cat /tmp/hosts > /mnt/c/Windows/System32/drivers/etc/hosts
else
    echo "$WINIP  winhost\n" >> /mnt/c/Windows/System32/drivers/etc/hosts
fi
cat /mnt/c/Windows/System32/drivers/etc/hosts
echo "DONE!\n"
```
cat /usr/local/bin/docker-start
```
#!/bin/bash
sudo prepare-hosts
sudo dockerd -H tcp://0.0.0.0:2375 -H unix:///var/run/docker.sock --dns 8.8.8.8
```

add it to service on boot, if needed