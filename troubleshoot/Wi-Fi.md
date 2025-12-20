
# Troubleshoot Wi-Fi 

[Linux Mint: Wi-Fi Adapter Not Detected(https://www.youtube.com/watch?v=ZZYQsjunjbs)

## Disable Power Saving
Disable WiFi power saving by modifying the NetworkManager configuration file :  

/etc/NetworkManager/conf.d/default-wifi-powersave-on.conf  
Change wifi.powersave = 3 (enabled) to > wifi.powersave = 2 (disable)

## Identify Wireless Network Adapter

Connect to internet via ethernet cable
`$ iwconfig`
   no wireless extensions

`$ lspci | grep -i wireless`
or 
`$ lspci | grep -i network`

If using a USB WiFi adapter :  
`$ lsusb`


## Install Appropriate Linux Driver

Install Broadcom driver (for BCM43xx chipsets):
`$ sudo apt install bcmwl-kernel-source`


## For Other Wireless Network Adapters

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

# Configure Wi-Fi
[How to configure WiFi with commands](https://www.youtube.com/watch?v=cwLRfr2_ers)

nmcli : Network Manager common line interface

## Ensure NetworkManager is running
```bash
$ sudo systemctl status NetworkManager
$ sudo systemctl start NetworkManager
```

## Check device status
```bash
$ nmcli device status
```
> UNAVAILABLE

## Ensure device is managed
```bash
$ nmcli device set wlp3s0 managed yes
```

## Turn wifi on
```bash
$ nmcli radio wifi on
$ nmcli device status
```
> DISCONNECTED


## List available access points
```bash
$ nmcli device wifi list
```
## Connect to WiFi

```bash
$ sudo nmcli dev wifi connect <SSID> password <pwd>
# Check 
$ nmcli device status
```
## To disconnect
```bash
$ nmcli device disconnect wlp3s0
```
---

# Script to diagnose Wi-FI 

```bash

#!/usr/bin/bash

echo "🔍 Diagnostic Wi-Fi (NetworkManager) — $(date)"
echo "=============================================="

# 1. Vérifier si wlan0 (ou toute interface Wi-Fi) existe
WIFI_IFACE=$(nmcli -t -f DEVICE,TYPE device status | grep wifi | cut -d: -f1 | head -n1)

if [ -z "$WIFI_IFACE" ]; then
    echo "❌ Aucune interface Wi-Fi détectée par NetworkManager."
    echo "   → Vérifiez le pilote ou le matériel (lspci, lsusb, dmesg)."
    exit 1
else
    echo "✅ Interface Wi-Fi détectée : $WIFI_IFACE"
fi

# 2. État actuel de l'interface
STATE=$(nmcli -t -f STATE device show "$WIFI_IFACE" 2>/dev/null | head -n1)

echo "📊 État actuel : $STATE"

if [ "$STATE" = "unavailable" ]; then
    echo "⚠️  L'interface est dans l'état 'unavailable'. Diagnostic en cours..."

    # 3. Vérifier rfkill
    echo "📡 Vérification de rfkill..."
    if rfkill list wifi | grep -q "Soft blocked: yes"; then
        echo "   → Soft block détecté ! Tentative de déblocage..."
        sudo rfkill unblock wifi
        echo "   ✅ Soft block supprimé."
    else
        echo "   ✅ Pas de soft block."
    fi

    if rfkill list wifi | grep -q "Hard blocked: yes"; then
        echo "   ❌ Hard block détecté !"
        echo "   → Vérifiez le commutateur physique ou la touche Fn+F* sur votre clavier."
    fi

    # 4. Vérifier l'état de la radio Wi-Fi
    echo "📶 Vérification de la radio Wi-Fi..."
    if nmcli radio wifi | grep -q "enabled"; then
        echo "   ✅ Radio Wi-Fi activée."
    else
        echo "   → Radio Wi-Fi désactivée. Activation en cours..."
        nmcli radio wifi on
        echo "   ✅ Radio Wi-Fi activée."
    fi

    # Attendre un peu pour laisser le système réagir
    sleep 2

    # 5. Nouvel état
    NEW_STATE=$(nmcli -t -f STATE device show "$WIFI_IFACE" 2>/dev/null | head -n1)
    echo "🔄 Nouvel état après correction : $NEW_STATE"

    if [ "$NEW_STATE" = "disconnected" ] || [ "$NEW_STATE" = "connected" ]; then
        echo "🎉 Succès ! L'interface est désormais utilisable."
        echo "   → Pour vous connecter :"
        echo "      nmcli device wifi connect \"SSID\" password \"motdepasse\""
    elif [ "$NEW_STATE" = "unavailable" ]; then
        echo "❌ Toujours 'unavailable'. Problème probable :"
        echo "   - Pilote défectueux ou non chargé"
        echo "   - Problème matériel"
        echo "   → Vérifiez : dmesg | grep -i wifi"
    fi

elif [ "$STATE" = "disconnected" ]; then
    echo "💡 L'interface est prête. Scannez les réseaux avec :"
    echo "   nmcli device wifi list"
elif [ "$STATE" = "connected" ]; then
    CONN=$(nmcli -t -f CONNECTION device show "$WIFI_IFACE" | head -n1)
    echo "✅ Connecté au réseau : $CONN"
else
    echo "ℹ️ État inattendu : $STATE"
fi

echo "=============================================="

`` `
