# Linux Tips

- [Linux Tips](#linux-tips)
  - [Linux version](#linux-version)
  - [grub](#grub)
     + [Modify Options](#modify-options)
     + [grub-customizer](#Grub-Customizer)
  - [Reset password](#reset-password)
  - [Free up disk space](#free-disk-space)
  - [Move home into a separate partition](#move-home-into-a-separate-partition)
  - [Uninstall](#uninstall)
  - [Linux file system structure](#linux-file-system-structure)
  - [unbound](#local-dns-unbound)
  - [OpenVPN](#openvpn)
  - [Create shortcut on desktop](#create-shortcut-on-desktop)
  - [Enable WiFi](#enable-wifi)
  - [No Numpad](#no-numpad)
  - [Test Microphone](#test-microphone)
  - [Digest](#digest)
  - [Dates](#dates)
  - [File manipulations](#file-manipulations)
  - [Image manipulations](#image-manipulations)
  - [Sort images by orientation ](#sort-images-by-orientation)
  - [Text manipulations](#text-manipulations)
  - [Ssh](#ssh)
  - [StarDict + wiktionnaire](#stardict--wiktionnaire)
  - [Calibre](#calibre)
  - [wget](#wget)
  - [Compute Time duration](#compute-time-duration)
  - [ffmpeg](#ffmpeg)
  - [Basic calculation](#basic-calculation)
  - [Gimp](#gimp)
  - [youtube-dl](#youtube-dl)
  - [HP Printer](#hp-printer)
  - [GPG error](#gpg-error)
  - [Sublime Text](#sublime-text)
  - [Curl to test Rest API :](#curl-to-test-rest-api-)
  - [jq : json query](#jq--json-query)
  - [iconv](#iconv)
  - [Nintendo Switch Pro Controller on Linux](#Nintendo-Switch-Pro-Controller-on-Linux)
  - [cygwin tips ](#cygwin-tips)
   * [Install Cygwin without admin rights](#install-cygwin-without-admin-rights)
   * [cygwin setup (auto update)](#cygwin-setup-auto-update)
   * [Call bash script from cmd :](#call-bash-script-from-cmd-)
   * [Cygwin in contextual menu  ](#cygwin-in-contextual-menu)
   * [Copy Paste issue](#copy-paste-issue)
   * [Customize cygwin prompt](#customize-cygwin-prompt)
      + [Manually (buggy)  ](#manually-buggy)
      + [using git-prompt.sh (safer option)](#using-git-promptsh-safer-option)
   * [Keyboard shortcuts  ](#keyboard-shortcuts)
   * [cygpath](#cygpath)
   * [find : Permission denied](#find-permission-denied)
   * [ls : ignore case ](#ls-ignore-case)
   * [Files names containing spaces](#files-names-containing-spaces)
   * [mysql](#mysql)
   * [/dev/null ](#devnull)
   * [Generate new GUID ](#generate-new-guid)



---  

## Linux version
`$ uname --all`  or  `$ cat /etc/os-release`  or `$ lsb_release -a`    

## sysinfo
`$ inxi -F`  

Memory :  
`$ dmidecode --type memory`  

## Grub

### Modify options

Collect infos  
`sudo nano /boot/grub/grub.cfg`

Modify options  
> GRUB_DEFAULT=0 
> GRUB_TIMEOUT=5 
`sudo nano /etc/default/grub`  

Apply modifications    
`sudo update-grub`  

### Grub-Customizer
How to change Default kernel version &rarr; [grub-customizer](https://tipsonubuntu.com/2018/03/11/install-grub-customizer-ubuntu-18-04-lts/)    
`$ sudo apt-get install grub-customizer`   

> General Settings > default entry > predifined  
> Simply select the desired kernel in the drop-down list.  

Uninstall :   
`$ sudo apt-get remove --autoremove grub-customizer`   

## Reset-password

grub menu -> 'e' edit linux entry  
append : quiet splash **rw init=/bin/bash**  
boot '<F10>'  


## Free up disk space

Remove no longer needed packages :   
`$ sudo apt-get autoremove`  

Remove old kernels :  
`$ sudo apt-get autoremove --purge`  


## Move home into a separate partition 

With a LiveCD  
- use **gparted** to reduce root partition (/dev/sda5) and create a new one for /home  (/dev/sda6)
- mount and rsync the original /home directory to new /home directory  
```bash
$ sudo mkdir /mnt/root
$ sudo mkdir /mnt/home
$ sudo mount /dev/sda5 /mnt/root
$ sudo mount /dev/sda6 /mnt/home
```
```bash
# rsync options source destination
# -a : archive mode (preserve original permissions!)  
# -v : verbose  
# -z : compress during transfer  
# -h : human readable final report  
$ rsync -avzh /mnt/root/home/ /mnt/home/
$ sudo mv /mnt/root/home/ /mnt/root/home-old/
```

Note the UUID of new home partition /dev/sda6
`$ blkid`

Edit /mnt/root/etc/fstab to automount after restart
`$ sudo nano /mnt/root/etc/fstab` :   
```
 <file system> <mount point>   <type>  <options>       <dump>  <pass>
# New home partition
UUID=<XXXX>	/home	ext4	defaults,nofail	0	0 
```

- Restart without LiveCD and check it's OK  

- Retake the ownership of your home directory  
`$ sudo chown -R $USER:$USER /home/$USER`  

- And remove the /home-old directory  
`$ sudo rm -rf /home-old`


## Uninstall 

from apt-get  
`$ apt list --installed`  
`$ sudo apt-get remove --purge dotnet-sdk-6.0`  

Uninstall .NET  
`$ sudo apt-get purge dotnet-sdk-*`  
`$ sudo apt-get purge dotnet-runtime-*`  
or  
`$ sudo apt remove --purge dotnet-sdk-6.0`  
`$ sudo apt remove --purge dotnet-runtime-6.0`  


from source  
`$ make uninstall` or `sudo make uninstall`    

## Linux file system structure

[youtube](https://www.youtube.com/watch?v=HbgzrKJvDRw)  
[geeksforgeeks](https://www.geeksforgeeks.org/linux-file-hierarchy-structure/)  
![image](images/LinuxFileSystem.jpg)  

## Local Dns Unbound

Installer le paquet Unbound :  
`$ sudo apt-get install unbound`  

Téléchargement de la liste des serveurs DNS racines :  
`$ cd /var/lib/unbound/ && wget ftp://ftp.internic.net/domain/named.cache`  

Mise en place de cette liste pour le serveur Unbound  
`$ mv named.cache root.hints && chown unbound:unbound root.hints`  

Fichier de configuration  
/etc/unbound/unbound.conf  

```bash
server:
# Activer les logs
verbosity: 1
interface: 127.0.0.1
port: 53
do-ip4: yes
do-ip6: no
do-udp: yes
do-tcp: yes
access-control: 192.168.1.0/24 allow
#emplacement du fichier serveurs DNS root
#fichier à télécharger là à cette adresse: ftp://ftp.internet.net/domain/named.cache
root-hints: "/var/lib/unbound/root.hints"
# Duplicate with /etc/unbound/unbound.conf.d/root-auto-trust-anchor-file.conf
#auto-trust-anchor-file: "/var/lib/unbound/root.key"
#Cacher les infos sur le serveur DNS
hide-identity: yes
hide-version: yes
#paramètre limitant l'usurpation de DNS
harden-glue: yes
#Requérir les infos DNSSEC pour les zones de confiance
harden-dnssec-stripped: yes
use-caps-for-id: yes
# Bloquer cetaines pubs
local-zone: "doubleclick.net" redirect
local-data: "doubleclick.net A 127.0.0.1"
local-zone: "googlesyndication.com" redirect
local-data: "googlesyndication.com A 127.0.0.1"
local-zone: "googleadservices.com" redirect
local-data: "googleadservices.com A 127.0.0.1"
local-zone: "google-analytics.com" redirect
local-data: "google-analytics.com A 127.0.0.1"
local-zone: "ads.youtube.com" redirect
local-data: "ads.youtube.com A 127.0.0.1"
local-zone: "adserver.yahoo.com" redirect
local-data: "adserver.yahoo.com A 127.0.0.1"
local-zone: "ask.com" redirect
local-data: "ask.com A 127.0.0.1"
forward-zone:
name: "."
forward-addr: 208.67.222.222 #serveur DNS OpenDNS
forward-addr: 208.67.220.220 #serveur DNS OpenDNS
forward-addr: 8.8.8.8 #serveur DNS de google
forward-addr: 8.8.4.4 #serveur DNS de google
```

Relance du service Unbound :  
`$ systemctl restart unbound`

Vérifier le status :  
`$ systemctl status unbound`  

Vérifier la résolution de nom à partir du serveur :  
`$ sudo unbound-checkconf`  

Extra blacklist:  
[How to Block Ads with Unbound DNS Server](https://infotoast.org/site/index.php/2023/12/07/how-to-block-ads-with-unbound-dns-server/)  
```
$ curl -o block.conf https://infotoast.org/Downloads/hosts/unbound.php
$ sudo cp block.conf /etc/unbound/unbound.conf.d/block.conf
``` 
Or [Using Unbound as an Ad-blocker](https://wiki.alpinelinux.org/wiki/Using_Unbound_as_an_Ad-blocker)  
```
$ sudo echo "server:" >/etc/unbound/unbound.conf.d/blacklist.conf
$ sudo curl -s https://raw.githubusercontent.com/StevenBlack/hosts/master/hosts | \
grep ^0.0.0.0 - | \
sed 's/ #.*$//;s/^0.0.0.0 \(.*\)/local-zone: "\1" refuse/' \
>>/etc/unbound/unbound.conf.d/blacklist.conf
``` 
Update unbound:   
```bash
sudo service unbound stop
sudo wget ftp://FTP.INTERNIC.NET/domain/named.cache -O /var/lib/unbound/root.hints
sudo service unbound start
```

## OpenVPN

**Download and unzip *.ovpn**  
Choose an country, Download and unzip [vpnbook bundle](https://www.vpnbook.com/freevpn)  
note the username and password 

**Launch openvpn** 
`$ sudo openvpn --config '~\Downloads\vpnbook-ca149-tcp443.ovpn'`

**Deactivate IPv6**  
``` 
$ sudo sysctl -w net.ipv6.conf.all.disable_ipv6=1
$ sudo sysctl -w net.ipv6.conf.default.disable_ipv6=1
$ sudo sysctl -w net.ipv6.conf.lo.disable_ipv6=1
```
Check your IP : https://www.whatismyip.com/  

## Create shortcut on desktop
<Ctrl+Shift> Drag/Drop on desktop  

## Browse from Computer Level
URI : `computer:///`  

## Enable WiFi

[How to enable Wifi drivers in Linux](https://www.youtube.com/watch?v=X2rFkqyD0oY)  

Collect informations  
```bash
$ ifconfig -a

$ ip a

$ iwconfig

```

Collect Drivers Infos

```bash
# -knn : kernel, vendor, vendor codes
$ lspci -knn | grep Network -A5

	00:14.3 Network controller [0280]: Intel Corporation Device [8086:43f0] (rev 11)
		DeviceName: Onboard - Ethernet
		Subsystem: Intel Corporation Device [8086:0074]
		Kernel driver in use: iwlwifi
		Kernel modules: iwlwifi
	...

```
OR

```bash
$ sudo lshw -class network
	*-network                 
       description: Wireless interface
	...
       logical name: wlo1
       ...

```
OR

```bash
$ inxi -N
	Network:   Device-1: Intel driver: iwlwifi 
	           Device-2: Realtek RTL8111/8168/8411 PCI Express Gigabit Ethernet driver: r8169 

```

Check if modules are loaded into the system  
lsmod - shows the status of modules in the kernel  

```bash
$ lsmod | grep -e wl -e iwlwifi
	...
	iwlwifi               335872  1 iwlmvm

# modprobe - add and remove modules from the kernel
$ sudo modprobe iwlwifi

$ lsmod | grep -e wl -e iwlwifi

```


## Scroll bars visible

[Make scroll bars visible](https://www.technipages.com/linux-mint-make-scroll-bars-visible/)
Themes > Settings :  
![image](images/mint-scroll-bars.png)  

## No Numpad

install autokey-gtk : to re-assign keys combination (eg : Alt+W = 1, Alt+X = 2, etc.)  

## Test Microphone

`$ arecord -d 5 test.wav`  
`$ aplay test.wav`  

## Digest  

sha1sum, md5sum, etc.
`$  sha1sum file.txt | tee file.txt.sha1`  
> 362de0c3b8829da225431d40a82606808da39f68 *file.txt  

`$ sha1sum -c file.txt.sha1'  
> file.txt : OK  


## Dates

### Date format
Install missing locale if needed :  
`$ mintlocale`  
Define default time format :  
`$ echo "LC_TIME=fr_BE.UTF-8" >> $HOME/.profile`

### arithmetics  
Addition  
`$ date --date="1 month" +"%d-%m-%Y" `  
> 02-04-2023  

Substraction  
`$  date --date="15 days ago" +"%d-%m-%Y"`  
> 15-02-2023  

### Get lastdate of a month
`$ date --date="20040201 +1 Months -1 days" +%Y%m%d` 


### Count working days (excluding WE)

`$ cal 2 2016 | tail -n +3 | cut -c 3-17 | tr ' ' '\n' | grep -v '^$' | sort -n | wc -l | head -n 1`

### Time duration

`$ date +%H:%M:%S -ud $(echo @$(expr $(date +%s -d 06:46:00) - $(date +%s -d 05:36:11)))`

> Output : 01:09:49

---
## File manipulations  

### List only directories :  
`$ ls -d */`  
or  
`$ find . -type d -maxdepth 1`  

"find *" instead of "find ." to remove leading dot :  

`$ find * -iname "*.puml" -exec echo {} \;`  

Drawbacks :   
- hidden files omitted at top level   
- not working with many many files
- not working with files starting with `!, -, (, )`  

### Find all files create|modified|accessed on a given date  
- ct : creation  
- mt : modification  
- at : access  
`$ find . -type f -newermt 2024-12-12 ! -newermt 2024-12-13`  

### Ignore case with ls :  
`$ shopt -s nocaseglob; ls -1 *.pdf`  

For loop with multiple file extensions :  
```bash
shopt -s nullglob
for file in ./*.{txt, md, html}
do
   echo "$file"
done
shopt -u nullglob 
```

### Change modification date :  
`$ touch -d "2004-02-29 12:34" filename` 

### Find file modified less than a day ago :  
`$ find . -name '*.csv' -ctime -1`  

### Replace text in multiple files :   
`$ sed -i 's/old-text/new-text/g' *.* `

### Remove empty lines with sed :  
`$ sed -r '/^\s*$/d' file.txt` 

### Insert one line at the top of a file :  
`$ sed -i '1 i EXTRA_TOP_LINE'` 

### Append one line at the end of a file :  
`$ sed -i '$ a EXTRA_BOTOOM_LINE'`  

### Insert 'commit;' every 1000 lines
```bash
#!/usr/bin/bash
if [ $# -ne 1 ]; then
  echo "Usage: $0 input_file"
  exit 1
fi
input_file="$1"

total=$(wc -l $input_file | cut -f 1 -d' ')
echo "Total lines : $total"

subtotal=$total
slice=1000
while [ $subtotal -gt $slice ] ; do
	subtotal=$(($subtotal-$slice))
	cat $input_file | sed -i "${subtotal}a commit;" $input_file
done
new_total=$(wc -l $input_file | cut -f 1 -d' ')
echo "Total lines : $new_total"

echo "Terminated"

```
### Split into smaller files :  
```
# -l : nb lines
# -d : add autonum suffix
# -a : nb of digits for suffix
$ split -l 100 input.sql "baseFileName" -d -a 3 --additional-suffix=".sql"
```

### Renaming files + autonumber suffix :  
`$ rename  's/.+/our $i; sprintf("MyGallery_%03d.jpg", 1+$i++)/e' *`

### Renaming files + format number :
`for file in $(ls -1 *.jpg | sort -n) ; do rename  -- 's/(\d+)/sprintf "%03d", $1/e' $file ; done`  

### Rename files extensions :  
`$ find . -name "*.old" -exec bash -c 'mv "$1" "${1%.old}".new' - '{}' \;`  
or  
`$ find . -iname "*.old" -exec rename .old .new {} \;`   

### Counting records in csv files for given field :  
```bash
$ for file in $(ls *.csv) ; do echo "$file $(echo `wc -l $file | cut -f1 -d';' | bc`-1 | bc)" ; done`
```


### Skip 1st line :  
`$ tail -n + 2 filename`


### GREP : speed up research by targeting files extension and by excluding folder
`$ grep -nr "private static final" --include=\*.java --exclude=target` 


### GREP : Get lines Before|After pattern   
`$ grep -B 500 'sometext' file.txt` 
`$ grep -A 500 'sometext' file.txt` 

### GREP : extract substring between double quotes   
option : -o, --only-matching  
`$ grep -o '".*"' input.txt | tr -d '"' | sort -u`


### Find lines in common between 2 SORTED files :  
`$ join file1.txt file2.txt`  

### Find lines not in common with grep :  
-v : invert matching  
`$  grep -vf file1.txt file2.txt ` only in file1.txt   
`$  grep -vf file2.txt file1.txt ` only in file2.txt  


## Image manipulations

### Sort images by orientation
```bash
#!/bin/bash
# use imagemagick's identify with the fx special operator
# to compare height and width eg. check h/w ratio

mkdir -p ./portrait
mkdir -p ./landscape

shopt -s nullglob # to avoid error with extension not found
shopt -s nocaseglob # case insensitive

for file in ./*.{jpg,jpeg,png}
do
	ratio=$(identify -format '%[fx:(h>w)]' "$file")
	if [[ ratio -eq 1 ]] 
	then
		mv "$file" ./portrait/
	else
		mv "$file" ./landscape/
	fi
done

shopt -u nullglob # unset nullglob
shopt -u nocaseglob # unset nocaseglob

```

### How to crop automatically image 
Trim white regions away with imagemagick :   

`$ convert -trim input.png output.png`  

## Text manipulations  

### Remove last character in file :  
`$ sed -i '$ s/.$//' filename`  

### Surrounding "[...]" :  
`$ sed -i '1 i [' inputfile && sed -i '$ a ]' outputfile`  

### Convert jdbc dates "{d 'yyyy-mm-dd'}" => 'yyyy-mm-dd' :  
`$ sed -i -E "s/\{d ('.{10}')\}/\1/g" file.sql`  

### Replace text in multiple files
`$ find . -name pom.xml -exec sed -i -e 's/\-before/\-after/g' {} \;`

### Swap 2 consecutive lines
Test 
`$ perl -0777 -pne 's/(.*pattern-line-A.*)\n(.*pattern-line-B.*)/$2\n$1/g' input.txt`  

Apply  
`$ perl -0777 -i.bak -ne 's/(.*pattern-line-A.*)\n(.*pattern-line-B.*)/$2\n$1/g' input.txt` 

### Sed multi-line mode :  
`sed -e '1h;2,$H;$!d;g' -e 's/\n/\t/g'`  

```
# 1h: put first line in the "hold" space (sed has 2 spaces: 1 hold space to keep data and the pattern space: actual processed line)  
# 1d: delete first line  
# $!H: append all lines BUT the last one (and the first one since d command skips to the next line) into the "hold" space  
# $!d: delete (do not print) all lines except the last one  
# g: Append a newline to the contents of the pattern space  
```

`$echo -e "Lorem ipsum\r\ndolor sit amet\r\nconsectetur adipiscing elit" | sed -e '1h;2,$H;$!d;g' -e 's/\r\n/;/g'`   
> Lorem ipsum;dolor sit amet;consectetur adipiscing elit

Alternative : 'N;s/pattern/replace/;P;D'  
```
# "N" read next line too
# "P" print result 
# "D" delete red next line (already red).
```



### Append string "commit" in each file  
`$ for file in $(ls -1 *.sql) ; do sed -i '$ a commit;' $file ; done`  

### Append string "--EOF" in each file
`$ for file in $(ls -1 *.sql) ; do sed -i '$ a --EOF' $file ; done` 

### Unwanted characters...  
`$ iconv --unicode-subst="<U+%04X>" -f utf8 -t ascii input.xsd | tee temp/charset.xsd`  
or trying to remove accents...  
`$ iconv -f utf8 -t ascii//TRANSLIT input.xsd | tee temp/iconv_translit.xsd`   

### Convert non-ascii characters as unicode escape sequence  
- IntelliJ plugin : Unicodifier  
- VSCode plugin : ascii-unicode-escape  
- iconv -t java   

```
# original text
$ cat messages.txt
mail.body=Les dossiers enrôlés seront débloqués au plus tôt 2 jours ouvrables après la date exécutoire et les
dossiers non-enrôlés seront libérés demain en matinée. Les enrôlements prochains sont mentionnés
sur l'intranet / BizTax Gestion et BizTax Consultation/ Documentation/ Calendriers
```

```
# convert non-ascii -> unicode
$ iconv -t java messages.txt
mail.body=Les dossiers enr\u00f4l\u00e9s seront d\u00e9bloqu\u00e9s au plus t\u00f4t 2 jours ouvrables apr\u00e8s la date ex\u00e9cutoire et les
dossiers non-enr\u00f4l\u00e9s seront lib\u00e9r\u00e9s demain en matin\u00e9e. Les enr\u00f4lements prochains sont mentionn\u00e9s
sur l'intranet / BizTax Gestion et BizTax Consultation/ Documentation/ Calendriers
```

### Display non control characters

`$  echo "hello" | od -c | sed -E 's/^[0-9]{7}//;s/ //g'`  


### Read-Write permissions  
`$ find . * -exec chmod u+rwx {} \;`  
(cygwin : setfacl)  
`$ find . -exec setfacl -s user::rw-,group::r--,other::r-- {} \;`  

### Mimic a "GROUP BY HAVING field=MAX()" using tail, sort (much faster than SQL on large tables)  
```
# sort options :
# -k : -k, --key=POS1[,POS2] 	Start a key at POS1 (origin 1), end it at POS2 
# -r --reverse
# -n, --numeric-sort
# -o, --output=FILE
# -t, --field-separator=SEP

# Example 
# Group By Having field3=Max(field3)' per distinct couples field1-field2
$ tail -n +2 input.csv | sort -k3,3r | sort -k1,1 -k2,2 -u -o output.csv

# sort -k3,3r : sort on 3th field in reverse order
# sort -k1,1 -k2,2 -u : sort uniques couples 1st-2d fields... 
# good to know : "sort" seems to keep the first occurence of the 3th field
```

---

## ssh

Activate ssh :  

```bash
$ sudo apt-get install openssh-server
$ systemctl status sshd
$ systemctl enable sshd
```
```bash
$ ssh-keygen -t rsa
[or ssh-keygen -t rsa -C user.name@gmail.com]
[...]
The key fingerprint is:
SHA256:................................. username@local_hostname
The key's randomart image is:
+---[RSA 2048]----+
|       ..  oo.   |
|      E.o  .. o  |
|       ooO.  . . |
|       .@o* . o  |
|     . +S* o +   |
|      +o+.= . o  |
|     . =.= o   o |
|    . oo.++     .|
|     +o+Oo..     |
+----[SHA256]-----+
$ ssh-copy-id -i .ssh/id_rsa.pub username@remote_hostname
```
port 22: connection refused :  
ufw on remote host  
```
$ sudo ufw status
$ sudo ufw allow 22
$ sudo ufw reload
ssh-copy-id -i .ssh/id_rsa.pub username@remote_hostname
```

Copy file to remote host (eg : 192.168.0.3) :  

`$ scp file.zip user@192.168.0.3:/home/user/Desktop/`  

Copy directory to remote host :   

-r : recursive, -p : preserve properties
`$ scp -r /local/directory/ user@192.168.0.3:/home/user/Desktop/`  

Check file modification date on remote :  

`$ ssh -i /cygdrive/c/cygwin64/home/username/.ssh/id_rsa username@ftpserver stat ./folder/file.xlsx | grep "Modify"`    

---

## StarDict + wiktionnaire

- Download and install StarDict (https://sourceforge.net/projects/stardict-4/files/3.0.6.2/stardict_3.0.6.2-1_all.deb/download)

stardict_3.0.6.2-1_all.deb

- [Download dictionaries] (https://github.com/BoboTiG/ebook-reader-dict/releases/tag/fr)
[wikdict.com](https://download.wikdict.com/dictionaries/)  

- Unzip and copy dict-fr into stardict folder

`$ sudo cp -r dict-fr /usr/share/stardict/dic/`

- Install goldendict (based on stardic)
  
`$ sudo apt-get install goldendict`  

---

## Calibre

https://calibre-ebook.com/download_linux

[Download calibre 4.23.0](https://download.calibre-ebook.com/4.23.0/calibre-4.23.0-x86_64.txz)

install calibre tarball

```
$ sudo mkdir -p /opt/calibre && \  
rm -rf /opt/calibre/* && \  
sudo tar xvf /home/jinx/Downloads/calibre/calibre-4.23.0-x86_64.txz -C /opt/calibre && \  
sudo /opt/calibre/calibre_postinstall
```

[Download and unzip DeDRM_tools 6.8.1-including Obok plugin](ttps://github.com/apprenticeharper/DeDRM_tools/releases/download/v6.8.1/DeDRM_tools_6.8.1.zip)  
[Fork](https://github.com/noDRM/DeDRM_tools)  
Load plugin DeDRM in Calibre

> menu > preferences > advanced > plugins 


---

## wget 

Leech an entire web site

     --recursive: download the entire Web site.  
     --domains website.org: do not follow links outside website.org.  
     --no-parent: do not follow links outside the directory tutorials/html/.  
     --page-requisites: get all the elements that compose the page (images, CSS and so on).  
     --html-extension: save files with the .html extension.  
     --convert-links: convert links so that they work locally, off-line.  
     --restrict-file-names=windows: modify filenames so that they will work in Windows as well.  
     --no-clobber: do not overwrite any existing files (used in case the download is interrupted and resumed).  

```
$ wget \
     --recursive \
     --page-requisites \
     --html-extension \
     --convert-links \
     --restrict-file-names=windows \
     --domains example.com \
     --wait=3 \
     --no-parent \
	https://www.example.com/index.html
```
one-line :  
`$ wget --recursive --page-requisites --html-extension --convert-links --restrict-file-names=windows --domains example.com --wait=3 --no-parent  https://www.example.com/index.html`  


Basic authentication :  
`$ wget --user=username --password=superpasswd http://target-url`  

---

## Compute Time duration
```
START=$(date +%s -d 05:36:11)
END=$(date +%s -d 06:46:00) 
DURATION_SECONDS=$(expr $END - $START)
date +%H:%M:%S -ud @$DURATION_SECONDS
```
	Output : 01:09:49

oneline  

`date +%H:%M:%S -ud $(echo @$(expr $(date +%s -d 06:46:00) - $(date +%s -d 05:36:11)))` 


## ffmpeg

- Cut Sample Video (Fast)

38:43 : start  
04:26 : duration  
43:09 : end  

`$ ffmpeg -ss 00:38:43 -t 00:04:26 -i "input.avi" -acodec copy -vcodec copy sample.avi`

- VOB to mp4 (DVD lossless) 
`$ ffmpeg -i input.VOB -c:v copy -c:a copy output.mp4`   

- mp4 to gif

`$ ffmpeg -i input.mp4 -vf "fps=10,scale=320:-1:flags=lanczos" -c:v pam -f image2pipe - | convert -delay 10 - -loop 0 -layers optimize output.gif]` 

- [Screen recording](https://www.wikihow.com/Record-Your-Desktop-Using-FFmpeg-on-Ubuntu-Linux)

	no sound
`$ ffmpeg -video_size 1920x1080 -framerate 30 -f x11grab -i :0.0+0,0 -c:v libx264rgb -crf 30 -preset ultrafast sample.mkv`  

	with sound
`$ ffmpeg -video_size 1920x1080 -framerate 30 -f x11grab -i :0.0+0,0 -f pulse -filter_complex amerge -ac 2 -i 0 -f pulse -ac 2 -i 1 -c:v libx264rgb -crf 0 -preset ultrafast sample.mp4`  

<details>
	<summary>Details</summary>
[screen_size]:
	Find out screen size : `$ xdpyinfo | grep dimensions` 

[ffmpeg_params]:
	**-video_size**
	    specifies the size of the recorded area. 
	    If you have a different screen size, use that instead of 1920x1080. 
	    If you want to record only an area of the screen, specify the area size here.

	**-framerate** 
	    specifies the frame rate, i. e. how many frames of video are recorded in a second. 
	    If you need another frame rate, use another number than 30. 
	    The lowest allowed framerate is 20.

	**-f x11grab** 
	    is what actually tells FFmpeg to record your screen. You shouldn't change that.

	**-i :0.0+0,0** 
	    is where you specify the x and y offset of the top left corner of the area that you want to record. 
	    For example, use :0.0+100,200 to have an x offset of 100 and an y offset of 200.

	**-c:v libx264rgb -crf 0 -preset ultrafast**
	    are encoding options. These specify a fast and lossless recording.

	 **-f pulse** 
	    tells FFmpeg to grab the input from PulseAudio, which is your sound server.

	**-ac 2** 
	    specifies the number of audio channels. 
	    If you receive an error like: "cannot set channel count to 2 (Invalid argument)",
	    you should change that to 1.

	**-i 0** 
	    specifies which device to grab the input from. 
	    You can see a list of all devices with the command `pacmd list-sources`. 
	    The number behind -i is the index listed there. 
	    The other output of the command will give you an explanation of what that audio device is for. 
	    A device with a name like "Monitor of Built-in Audio Analog" will most likely record the system audio, 
	    while something with "microphone" in the description will most likely be a microphone.  
</details>

- Cygwin + microphone (first : enable "Stereo Mix" mmsys.cpl)  

`$ ffmpeg -list_devices true -f dshow -i dummy` 

`$ ffmpeg -f gdigrab -s 1920x1080 -i desktop -f dshow -i audio="Microphone Array (Realtek(R) Audio)" -crf 28  -filter:a "volume=1.5" -vcodec libx264 output.mp4`  


- Extract images from gif

[creatomate](https://creatomate.com/blog/how-to-extract-images-from-a-video-using-ffmpeg)  
[bannerbear](https://www.bannerbear.com/blog/how-to-extract-images-from-a-video-using-ffmpeg/)  

Get informations  
`$ ffmpeg -i input.gif`  

Basic extraction  
`$ ffmpeg -i input.gif frame%04d.png`  

Extract every seconds  
`$ ffmpeg -i input.gif -vf fps=1 frame%04d.png`  

Extract at given time  
`$ ffmpeg -ss 5 -to 8 -i input.gif frame%04d.png`  


---

## Slow down | Speed up GIF with ImageMagick  

`$ identify -verbose test.gif | grep Delay`  
> Delay: 10x200
10 hundredths of a second delay between each frame  

`$ convert -delay 20x200 test.gif test_slow.gif`   



---

## Basic calculation  

`$ echo "2^10" | bc`

---

## Gimp

[Launch Gimp in French](https://docs.gimp.org/en/gimp-fire-up.html#gimp-concepts-running-language)  

`$ LANGUAGE=fr gimp` 

---

## youtube-dl

`$ sudo apt-get install youtube-dl`  
`$ sudo apt-get install python3-pip python-dev`  


---

## HP Printer

`$ sudo hp-setup -i`

---

## GPG error

`$ sudo apt-get update`

"GPG error: http://dl.google.com stable Release: The following signatures couldn't be verified because the public key is not available: NO_PUBKEY 78BD65473CB3BD13"

`$ wget -q -O - https://dl.google.com/linux/linux_signing_key.pub | sudo apt-key add -`

---

## Sublime Text

[Get rid of nag screen](https://gist.github.com/tonkoandrew/6da28ad58ee2a0371f8019142fb898c1)

`$ cp remover.py ~/.config/sublime-text/Packages/User/`

Reformat/Reindent Code  

CTRL+SHIFT+P : to open Command palette  
S, S, J : Enter selected Java as current syntax  
CTRL+A : to select all code  
CTRL+SHIFT+P : to open Command palette again  
r, e, i, n, ENTER : to issue the reindent command


## Curl to test Rest API :  
```
$ curl -X POST -H "Content-Type:application/json" --data @example.json http://localhost:8080/rest/app/1/add 
{"field1":"1","field2":"2"}
```
## jq : json query 

Filter output for selected fields :  
`$ jq -r '[ .[] | {id: .id, field1: .field1} ]' file.json `  

Filter array by range :  
`$ jq -r '[ .[293:296] | .[] | {id: .id, field1: .field1} ]' file.json`  

Export to csv (problem if order of columns matters) :  

`$ jq -r -f filter.jq file.json > out.csv`  

```
 filter.jq :
def tocsv:
    (map(keys)
        |add
        |unique
    ) as $cols
    |map(. as $row
        |$cols
        |map($row[.]|tostring)
    ) as $rows
    |$cols,$rows[]
    | @csv;
tocsv
```
or (one-line)  
`$ jq 'map(. | .id, .field1, .field2 | tostring) | @csv' input.json > output.csv`  

`$ jq -r '.[] | [.field1, .field2, .field3] | @csv' input.json > output.csv`  

some arithmetic on fields :  
`$ jq -r '.[] | [.field1, .field2, (.fieldX | tonumber)+(.fieldY | tonumber)] | @csv ' input.json > output.csv`  

File encoding may prevent jq to work properly

## iconv

Check file encoding  

`$ file -i file.json` 

> file.json: text/plain; charset=utf-16le

Change file encoding    

`$ iconv -f UTF-16LE -t UTF-8 file.json > file-new.json`  

Or (with TRANSLIT)   

`$ iconv -f UTF-16LE -t UTF-8//TRANSLIT file.json > file-new.json`  

Or (script version)   

```
#!/usr/bin/bash
# Force file encoding to UTF-8
for file in $(ls *.json) 
do
	# get previous encoding by parsing file output
	# input.json: text/plain; charset=utf-16le
	ENC_FROM=$(file -i $file | cut -d';' -f2 | cut -d'=' -f2)	
	
	# //TRANSLIT if supported (cf. iconv version)
	ENC_TO=UTF-8//TRANSLIT
	iconv -f ${ENC_FROM} -t ${ENC_TO} $file > "${file%.json}_new.json"
	
	# Check result
	file -i "${file%.json}_new.json"
done
```

Or (by using vim in "ex" and "silen" mode)    

`$  vim -es '+set fileencoding=utf-8' '+wq!' file.json `  

---  
## keyboard

	*xev* : Press any key to identify the corresponding Keycode  

`$ xev | awk -F'[ )]+' '/^KeyPress/ { a[NR+2] } NR in a { printf "%-3s %s\n", $5, $8 }'`

	*xdotool* : Emulate key stroke

`$ xdotool key 5`  

	*xkeycaps* : graphically display and edit the X keyboard mapping

	*Xbindkeys* : to bind commands to certain keys or key combinations on the keyboard.
---

## Nintendo Switch Pro Controller on Linux 

`$ sudo nano /etc/udev/rules.d/60-steam-input.rules`  

```
# Nintendo Switch Pro Controller over USB hidraw
KERNEL=="hidraw*", ATTRS{idVendor}=="057e", ATTRS{idProduct}=="2009", MODE="0660", TAG+="uaccess"
# Nintendo Switch Pro Controller over bluetooth hidraw
KERNEL=="hidraw*", KERNELS=="*057E:2009*", MODE="0660", TAG+="uaccess"
```
Go to Bluetooth device manager ;-)  

---

<!-- TOC --><a name="cygwin-tips"></a>
## cygwin tips 

<!-- TOC --><a name="install-cygwin-without-admin-rights"></a>
### Install Cygwin without admin rights
`> setup-x86_64.exe --no-admin`  

<!-- TOC --><a name="cygwin-setup-auto-update"></a>
### cygwin setup (auto update)

<cygwin-path>\cygwin64\usr\local\bin\cygwin-setup.sh :  
```
#! /usr/bin/sh
rm setup-x86_64.exe
wget cygwin.com/setup-x86_64.exe
chmod u+x setup-x86_64.exe
run ./setup-x86_64.exe --no-admin
```
Create a shortcut in <cygwin-path> :  
`<cygwin-path>\cygwin64\bin\mintty.exe -i /Cygwin-Terminal.ico /bin/bash -l -c 'cygwin-setup.sh'`  

<!-- TOC --><a name="call-bash-script-from-cmd-"></a>
### Call bash script from cmd :

```cmd
@echo off
@%~d0
@cd %~p0
@C:\cygwin64\bin\bash -c ./my_script.sh
```

<!-- TOC --><a name="cygwin-in-contextual-menu"></a>
### Cygwin in contextual menu  
install **chere** package under the "Shells" category.
```
chere
-i : install  
-t : terminal  
-a : all users  
-e : contextual text  
-s : shell  
-p : print regtool commands to stdout rather than running them   
```
(as admin) `$ chere -i -e "Cygwin from here" -t mintty -s bash`  

cf new entry in contextual menu :   
> "&Cygwin from here" 

Customize contextual menu using registry 
> Modify contextual text (admin rights not needed):  

```bash
regtool -w -s set /HKCU/Software/Classes/Drive/Shell/cygwin64_bash '&Cygwin from here'
regtool -w -s set /HKCU/Software/Classes/Drive/Shell/cygwin64_bash/command 'C:\cygwin64\bin\mintty.exe -e /bin/xhere /bin/bash.exe "%V"'
regtool -w -s set /HKCU/Software/Classes/Drive/Shell/cygwin64_bash/Icon 'C:\cygwin64\Cygwin-Terminal.ico'
regtool -w -s set /HKCU/Software/Classes/Drive/Background/Shell/cygwin64_bash/command 'C:\cygwin64\bin\mintty.exe -e /bin/xhere /bin/bash.exe "%V"'
regtool -w -s set /HKCU/Software/Classes/Drive/Background/Shell/cygwin64_bash/Icon 'C:\cygwin64\Cygwin-Terminal.ico'

regtool -w -s set /HKCU/Software/Classes/Directory/Shell/cygwin64_bash '&Cygwin from here'
regtool -w -s set /HKCU/Software/Classes/Directory/Shell/cygwin64_bash/command 'C:\cygwin64\bin\mintty.exe -e /bin/xhere /bin/bash.exe "%V"'
regtool -w -s set /HKCU/Software/Classes/Directory/Shell/cygwin64_bash/Icon 'C:\cygwin64\Cygwin-Terminal.ico'
regtool -w -s set /HKCU/Software/Classes/Directory/Background/Shell/cygwin64_bash/command 'C:\cygwin64\bin\mintty.exe -e /bin/xhere /bin/bash.exe "%V"'
regtool -w -s set /HKCU/Software/Classes/Directory/Background/Shell/cygwin64_bash/Icon 'C:\cygwin64\Cygwin-Terminal.ico'

# You can remove them with this:
regtool -w unset /HKCU/Software/Classes/Drive/Shell/cygwin64_bash/Icon
regtool -w unset /HKCU/Software/Classes/Drive/Background/Shell/cygwin64_bash/Icon
[...]
```
<!-- TOC --><a name="copy-paste-issue"></a>
### Copy Paste issue

"\302\203": copy-to-clipboard  
"\302\226": paste-from-clipboard

Add the following line to your ~/.bashrc:

	stty lnext ^q stop undef start undef

And add the following line to your ~/.inputrc:

	"\C-v": paste-from-clipboard
[Infos](https://www.saltycrane.com/blog/2008/05/how-to-paste-in-cygwin-bash-using-ctrl/)  

<!-- TOC --><a name="customize-cygwin-prompt"></a>
### Customize cygwin prompt

<!-- TOC --><a name="manually-buggy"></a>
#### Manually (buggy)  

Edit .bashrc to define PS1 variable :   
```bash
# function to detect git repository
parse_git_branch() {
   git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/ (\1)/'
}
# Combine path + newline + git branch (if repository is present) + Customize colors...
# \[\e[1;33m\]\w\n	— current working directory in yellow + newline
# \[\e[32m\]\u@\h	— user and host in green
# \e[0;31m$(parse_git_branch)\[\033[37m\]	— git branch name in red + following in grey
export PS1="\[\e[1;33m\]\w\n\[\e[1;36m\]\[\e[0m\]\[\e]0;\w\a\]\[\e[32m\]\u@\h\e[0;31m$(parse_git_branch)\]\[\033[37m\] $ "
# OR
PS1=""
PS1="$PS1"'\[\e[1;33m\]\w'			# yellow working directory
PS1="$PS1"'\n'                  		# new line
PS1="$PS1"'\[\e[1;36m\]'        		# 
PS1="$PS1"'\[\e[0m\]'           		# 
PS1="$PS1"'\[\e]0;\w\a\]'       		# 
PS1="$PS1"'\[\e[32m\]'          		# change to green
PS1="$PS1"'\u@\h'               		# user@host
PS1="$PS1"'\e[0;31m$(parse_git_branch)\]'	# function to detect git repository
PS1="$PS1"'\[\033[37m\]'        		# 
PS1="$PS1"' $ '                 		# 
export PS1="$PS1"   
```
Reload .bashrc  
`$ source ~/.bashrc` 

![image](images/cygwin-prompt.png)  


<!-- TOC --><a name="using-git-promptsh-safer-option"></a>
#### using git-prompt.sh (safer option)

.bashrc :  
```
unset PS1
unset __git_ps1
source "/cygdrive/c/Program Files/Git/etc/profile.d/git-prompt.sh"
```

<!-- TOC --><a name="keyboard-shortcuts"></a>
### Keyboard shortcuts  
Alt + b : backward previous word  
Alt + f : forward  next word  
/cygdrive/c/  
/cygdrive/d/  

<!-- TOC --><a name="cygpath"></a>
### cygpath
cygpath -w <PATH> : windows like path  
```
$ cd /cygdrive/c/Windows/Temp
$ cygpath -w $(pwd)
C:\Windows\Temp
```
cygpath -u <PATH> : unix like path  
`$ cd "$(cygpath -u 'C:\Program Files\')"`  

<!-- TOC --><a name="find-permission-denied"></a>
### find : Permission denied
*find . -iname* : [get rid of polluting "Permission denied"](https://unix.stackexchange.com/questions/42841/how-to-skip-permission-denied-errors-when-running-find-in-linux#answer-42842)  

```bash
$ cd /cygdrive/c
$ find . -iname "filename" 2>&1 | grep -v "Permission denied"
```
<!-- TOC --><a name="ls-ignore-case"></a>
### ls : ignore case 
*ls* : how to ignore case :  
use shell options [shopts](https://www.cyberciti.biz/faq/ls-command-case-insensitive-mode-search-pattern-linux-unix/)  
`$ shopt -s nocaseglob ` -s : set ignore case  
`$ ls *.txt`  
`$ shopt -u nocaseglob ` -u : unset ignore case   

<!-- TOC --><a name="files-names-containing-spaces"></a>
### Files names containing spaces
Tackle Loop File Names with spaces : **IFS** (Internal Field Separator) :	
```bash
IFS_BAK=$IFS
IFS=$'\n'
for file in $(ls -1 Files/*.{pdf,PDF}) ;
do
   exiftool -Author -Creator -Title $file 
done
IFS=$IFSBAK	
```	

<!-- TOC --><a name="mysql"></a>
### mysql

After installation of mysql via Cygwin Setup  
```bash
# adapt config file etc/my.cnf.d/client.cnf
	[client]
	host=127.0.0.1
	protocol=tcp
	socket=/var/run/mysqld/mysqld.sock

# create folder needed by the server
$ mkdir /var/lib/mysql 

# creates the initial tables needed by mysql
$ mysql_install_db 

# start server
$ mysqld_safe &
```

[test_db](https://github.com/datacharmer/test_db)

```bash
$ mysql -u root -p < employees.sql
Enter password:
MariaDB [(none)]> show databases;
MariaDB [(none)]> use employees;
MariaDB [(none)]> show tables;
MariaDB [(none)]> select count(*) from Employee;
MariaDB [(none)]> quit;
```
databases folder : cygwin64/var/lib/mysql/  

<!-- TOC --><a name="devnull"></a>
### /dev/null 
Cygwin /dev/null equivalent :  
`$ wget http://download.thinkbroadband.com/1GB.zip -O NUL` 
```
db2 => connect to SAMPLE user username using password
db2 => load from c:\temp\csv\NUL of del replace into SCHEMA1.TABLE1 nonrecoverable
```

check db2_2.log Investigate wich columns are set to null : 
```
$ grep column db2.log | sed -e 's/.*column /column /;s/ cannot.*//' | sort -u
column 12  FIELD12
column 13  FIELD13
# these columns are ignored (null) - they are useless 
```
<!-- TOC --><a name="generate-new-guid"></a>
### Generate new GUID 
Generate "-r" random UUID :  
`$ uuidgen -r` 
> 5758c27e-0995-4f8f-aa8c-d269b773102a  

### 8080 Already in use  
`$ lsof -i:8080`  
`$ kill -9 <PID>`  

cygwin equivalent :  
`$ netstat -aonp tcp | awk  '$4 == "LISTENING" && $2 ~ /:8080$/ '`  

