this procedure is intended only for vm purpose, or other cases where the license is not needed

set custom kms server
````powershell
sudo slmgr /skms 192.168.1.70:1688
````
unistall it
````powershell
sudo slmgr /upk
````
clear it
````powershell
sudo slmgr /cpky
````
set it
````powershell
sudo slmgr /ipk 12345-12345-12345-12345-12345
````
activate it
````powershell
sudo slmgr /ato
````
!achtung: Change product key if needed

Check the Status of Windows Activation
````powershell
slmgr.vbs /dli
````