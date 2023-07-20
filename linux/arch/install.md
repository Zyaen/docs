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




````


````