[](https://github.com/orgs/linuxmint/discussions/902#discussioncomment-15203405)


zjays on Dec 9, 2025

Here is what I needed to do to get a Switch Pro Controller working on my Linux Mint install (version 22.2, which was upgraded from version 22.1 while staying on the 6.8 kernel):

1. Open the file /etc/bluetooth/input.conf for editing and uncomment the line #ClassicBondedOnly=true (make it ClassicBondedOnly=true instead). ([Credit](https://steamcommunity.com/app/221410/discussions/0/4630359652384705130/))
 
2. In order for motion/gyro to work properly (at least within Cemu), create the file /lib/udev/rules.d/60-steam-input.rules and enter the following inside it ([Credit](https://github.com/cemu-project/Cemu/issues/1097#issuecomment-1952626670)):

```
#1245  Nintendo Switch Pro Controller over USB hidraw
KERNEL=="hidraw*", ATTRS{idVendor}=="057e", ATTRS{idProduct}=="2009", MODE="0660", TAG+="uaccess"

# Nintendo Switch Pro Controller over bluetooth hidraw
KERNEL=="hidraw*", KERNELS=="*057E:2009*", MODE="0660", TAG+="uaccess"
```

3. In the Linux Mint Bluetooth GUI (Blueman manager), under adapter-->preferences, change the adapter name to "Nintendo" ([Credit](https://github.com/DanielOgorchock/linux/issues/33#issuecomment-2790851724))

4. Pair the controller using the terminal (instead of within Blueman manager) as follows ([Credit](https://github.com/bluez/bluez/issues/673#issuecomment-1849132576)):

```bash
$ bluetoothctl
$ scan on
# wait a second

$ devices
# copy the MAC of the device that you want to pair

$ pair MAC_ADDRESS
$ connect MAC_ADDRESS
$ trust MAC_ADDRESS
```

Notes:  

> Without doing step 4 (and possibly step 3?), the controller could be paired using the Blueman manager, but after every reboot or controller disconnection, the controller would need to be unpaired and re-paired again to work.
> 
> Instead of step 2, installing joycond and cemuhook (https://github.com/joaorb64/joycond-cemuhook) could be done to get motion control working in Cemu. However, I found that this method caused the vertical motion control to be faulty or at least not to my liking (instead of my expectation of tilting the whole controller up and down while keeping the sides level, vertical motion would only work by raising one side of the controller above the other).  

