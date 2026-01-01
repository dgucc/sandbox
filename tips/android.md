# Android

## Memo

- Activate Developper mode in the phone  
- Connect the phone to the computer  

Install ADB : Android Debug Bridge  
`sudo apt-get install android-tools-adb`  

Authorize access on the device  
`adb devices`  
* daemon not running; starting now at tcp:5037  
* daemon started successfully  
List of devices attached  
XXX	unauthorized

List of devices attached
XXX	device


adb shell — access to the shell on your device.  
adb shell [command] — Runs the specified command on your device.  
`adb shell`  

adb pull [destination] [source] — Pulls from your device to your computer.  
`adb pull /sdcard/Download`  
`adb pull /sdcard/DCIM`  

adb push [source] [destination] — Pushes from your computer to your device.  


---

## References

[Utiliser ADB](https://linuxembedded.fr/2014/03/utiliser-adb)  
[Shell access via ADB](https://docs.ubports.com/fr/latest/userguide/advanceduse/adb.html)  
