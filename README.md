# powershell-commands

### Delete an entire folder recursively
```powershell
rm C:\path\to\delete -r -fo
```

### Hide a folder
```powershell
attrib +s +h D:/{{folderName}}
```

### Shutdown 1 minute after startup
```powershell
schtasks /create /tn "IMSORRY" /tr "shutdown -s -t 60" /sc ONLOGON 
```

### Run as admin
```powershell
powershell Start-Process {{processName}} -Verb runAs
```

### Wireless Wi-fi hotspot
Firstly, you need to check if your physical network card supports Hosted Networks:
```powershell
netsh wlan show drivers | select-string "Hosted network supported"
```
If you don’t have a network adapter that supports it, you will need buy a USB wireless adapter that does. Now in order to create a hosted network named hostednetwork with an SSID of `FREE_WIFI` and password `FSOCIETY`:
```powershell
netsh wlan set hostednetwork mode=allow ssid=FREE_WIFI key=FSOCIETY
```
Now in order to start this wireless access point, just type:
```powershell
netsh wlan start hostednetwork
```
In case you’d like to stop it:
```powershell
netsh wlan stop hostednetwork
```
### Dumping Wi-Fi Passwords
```powershell
$WirelessSSIDs = (netsh wlan show profiles | Select-String ': ' ) -replace ".*:\s+"
$WifiInfo = foreach($SSID in $WirelessSSIDs) {
    $Password = (netsh wlan show profiles name=$SSID key=clear | Select-String 'Key Content') -replace ".*:\s+"
    New-Object -TypeName psobject -Property @{"SSID"=$SSID;"Password"=$Password}
}  
$WifiInfo | ConvertTo-Json
```
