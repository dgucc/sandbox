# Games

## PS1 Emulator : Duckstation

[install on Linux Mint](https://hackernoon.com/how-to-install-duckstation-for-playing-ps1-games-on-linux)  
`$ sudo apt install flatpak`  
`$ flatpak install flathub org.duckstation.DuckStation`  

[install on Android](https://gamingonphone.com/guides/how-to-download-and-play-ps1-games-on-android-using-the-duckstation-emulator/)  


## Nintendo Switch Pro Controller on Linux

`$ sudo nano /etc/udev/rules.d/60-steam-input.rules` 

```
# Nintendo Switch Pro Controller over USB hidraw
KERNEL=="hidraw*", ATTRS{idVendor}=="057e", ATTRS{idProduct}=="2009", MODE="0660", TAG+="uaccess"
# Nintendo Switch Pro Controller over bluetooth hidraw
KERNEL=="hidraw*", KERNELS=="*057E:2009*", MODE="0660", TAG+="uaccess"
```

Go to Bluetooth device manager ;-)
