# Android

## Memo

- Activate Developper mode in the phone  
- Connect the phone to the computer  

Install ADB : Android Debug Bridge  
`sudo apt-get install android-tools-adb`  

Authorize access on the device  
`adb devices`  

`adb shell`  

adb pull [destination] [source] — Pulls from your device to your computer.  
`adb pull /sdcard/Download`  
`adb pull /sdcard/DCIM`  

adb push [source] [destination] — Pushes from your computer to your device.  


---

## References

[Utiliser ADB](https://linuxembedded.fr/2014/03/utiliser-adb)  
[Shell access via ADB](https://docs.ubports.com/fr/latest/userguide/advanceduse/adb.html)  
