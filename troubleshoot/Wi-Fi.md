[Linux Mint: Wi-Fi Adapter Not Detected(https://www.youtube.com/watch?v=ZZYQsjunjbs)

# Identify Wireless Network Adapter

Connect to internet via ethernet cable
`$ iwconfig`
   no wireless extensions

`$ lspci | grep -i wireless`
or 
`$ lspci | grep -i network`

If using a USB WiFi adapter :  
`$ lsusb`


# Install Appropriate Linux Driver

Install Broadcom driver (for BCM43xx chipsets):
`$ sudo apt install bcmwl-kernel-source`


# For Other Wireless Network Adapters

|Vendor|Common Chipset|Linux Driver|
|---|---|---|
|Intel|Wi-Fi 6 AX200/AX201<br>Wi-Fi AC 7260/8260|iwlwifi|
|Realtek|RTL8821CE, RTL8723DE<br>RTL8111/8168/8411 LAN|rtl8821ce, r8723de<br>r8169 (kernel) / r8168 (DKMS)|
|Qualcomm| QCA9377, QCA6174|ath10k_pci|
|Atheros|AR9285, AR9485|ath9k|
|MediaTek|MT7601U, MT7921|mt7601u, mt7921e|
|Ralink|RT3290, RT5370|rt2800pci, rt2800usb|
|Broadcom|BCM4311, BCM4312, BCM4360|wl (proprietary), <br>brcmsmac (open), b43 (open)|



---

[How to configure WiFi with commands](https://www.youtube.com/watch?v=cwLRfr2_ers)

# Ensure NetworkManager is running
```bash
$ sudo systemctl status NetworkManager
$ sudo systemctl start NetworkManager
```

# Check device status
```bash
$ nmcli device status
```
> UNAVAILABLE

# Ensure device is managed
```bash
$ nmcli device set wlp3s0 managed yes
```

# Turn wifi on
```bash
$ nmcli radio wifi on
$ nmcli device status
```
> DISCONNECTED


# List available access points
```bash
$ nmcli device wifi list
```
# Connect to WiFi

```bash
$ sudo nmcli dev wifi connect <SSID> password <pwd>
# Check 
$ nmcli device status
```
# To disconnect
```bash
$ nmcli device disconnect wlp3s0
```

