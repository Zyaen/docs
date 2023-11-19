

````
dism /Get-ImageInfo /ImageFile:${wimPath}
````

````
dism /Apply-Image /ImageFile:${wimPath} /Index:${wimIndex} /ApplyDir:${installPath} /Compact
````

````
bcdboot ${installPath}\windows /s o: /f UEFI
````
````
oobe\bypassnro
````
````
pnputil /i /a ${driverFile}
````
````
dism /Image:C:\ /Add-Driver /Driver:${driverPath} /Recurse
````