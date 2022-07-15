# Linux Tips

## Date format : 
Install missing locale if needed :  
`$ mintlocale`  
Define default time format :  
`$ echo "LC_TIME=fr_BE.UTF-8" >> $HOME/.profile`

---
## File manipulations  

Renaming files + autonumber suffix :
`$ rename  's/.+/our $i; sprintf("MyGallery_%03d.jpg", 1+$i++)/e' *`

Counting records in csv files for given field :  
`$ for file in $(ls *.csv) ; do echo "$file $(echo `wc -l $file | cut -f1 -d';' | bc`-1 | bc)" ; done`  

Skip 1st line :  
`$ tail -n + 2 filename`

Remove last character in file :  
`$ sed -i '$ s/.$//' filename`  

Surrounding "[...]" :  
`$ sed -i '1 i [' inputfile && sed -i '$ a ]' outputfile`  

Convert jdbc dates "{d 'yyyy-mm-dd'}" => 'yyyy-mm-dd' :  
`$ sed -i -E "s/\{d ('.{10}')\}/\1/g" file.sql`  

Split into smaller files :  
```
# -l : nb lines
# -d : add autonum suffix
# -a : nb of digits for suffix
$ split -l 100 input.sql "baseFileName" -d -a 3 --additional-suffix=".sql"
# append string "commit" in each file
$ for file in $(ls -1 *.sql) ; do sed -i '$ a commit;' $file ; done
# append string "--EOF" in each file
$ for file in $(ls -1 *.sql) ; do sed -i '$ a --EOF' $file ; done
```

Unwanted characters...
`$ iconv --unicode-subst="<U+%04X>" -f utf8 -t ascii input.xsd | tee temp/charset.xsd`  
or trying to remove accents...  
`$ iconv -f utf8 -t ascii//TRANSLIT input.xsd | tee temp/iconv_translit.xsd`   

Read-Write permissions  
`$ find . * -exec chmod u+rwx {} \;`  
(cygwin : setfacl)  
`$ find . -exec setfacl -s user::rw-,group::r--,other::r-- {} \;`  

---

## pdf

reduce pdf size with ghostscript :  
`$ gs -sDEVICE=pdfwrite -dPDFSETTINGS=/ebook -q -o output.pdf input.pdf`

pdf to text : install "poppler" to use pdfTotext :  
`$ pdftotext input.pdf output.txt` 

Enhance jpg scan => pdf :  
**noteshrink** [python]: Convert scans of handwritten notes to beautiful, compact PDFs  
```
$ git clone https://github.com/mzucker/noteshrink.git  
# Pre-requisites  
$ sudo apt install imagemagick  
$ pip install numpy  
$ pip install scipy  
$ pip install pillow  
# as root, edit /etc/ImageMagick-6/policy.xml :  
 <policy domain="coder" rights="read | write" pattern="PDF" />  
$ cd test/  
$ python3 noteshrink.py *.jpg  
# result page0000.png page0001.png ... output.pdf  
```

View and edit pdf's meta-data using exiftool :  
`$ exiftool document.pdf`  
`$ exiftool -Author="Mr Smith" -Creator="Mrs Smith" -Title="New Title" document.pdf`  
To install with cygwin, search for 'perl-Image-exiftool'  

---

## ssh

Activate ssh :  

```
$ sudo apt-get install openssh-server
$ systemctl status sshd
$ systemctl enable sshd
```
```
$ ssh-keygen -t rsa
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

Check file modification date on remote :  

`$ ssh -i /cygdrive/c/cygwin64/home/username/.ssh/id_rsa username@ftpserver stat ./folder/file.xlsx | grep "Modify"`    

---

## StarDict + wiktionnaire

- Download and install StarDict (https://sourceforge.net/projects/stardict-4/files/3.0.6.2/stardict_3.0.6.2-1_all.deb/download)

stardict_3.0.6.2-1_all.deb

- [Download dictionaries] (https://github.com/BoboTiG/ebook-reader-dict/releases/tag/fr)

- Unzip and copy dict-fr into stardict folder

`$ sudo cp -r dict-fr /usr/share/stardict/dic/`


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

[Download and unzip DeDRM_tools 6.8.1 (including Obok plugin)](ttps://github.com/apprenticeharper/DeDRM_tools/releases/download/v6.8.1/DeDRM_tools_6.8.1.zip)

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
     --no-clobber \
     --page-requisites \
     --html-extension \
     --convert-links \
     --restrict-file-names=windows \
     --domains website.org \
     --wait=3
     --no-parent \
			https://www.tutorialspoint.com/whiteboard.htm
```

Basic authentication :  
`$ wget --user=username --password=superpasswd http://target-url`  


---

## ffmpeg

- Cut Sample Video (Fast)

38:43 : start  
04:26 : duration  
43:09 : end  

`$ ffmpeg -ss 00:38:43 -t 00:04:26 -i "input.avi" -acodec copy -vcodec copy sample.avi`

- mp4 to gif

`$ ffmpeg -i input.mp4 -vf "fps=10,scale=320:-1:flags=lanczos" -c:v pam -f image2pipe - | convert -delay 10 - -loop 0 -layers optimize output.gif]` 

- Screen recording  

`$ ffmpeg -f x11grab  -s 1366x768 -i :0.0 -r 25 -vcodec libx264  output.mkv`  
`$ ffmpeg -f x11grab  -s 1366x768 -i :0.0 -r 25 -vcodec libx264  output.flv`  

---

## Basic calculation  

`$ echo "2^10" | bc`


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
`$ jq 'map(. | .id, .field1, .field2 | tostring) | @csv' input.json > out.csv`  

---

## cygwin tips 

Alt + b : backward previous word  
Alt + f : forward  next word  
/cygdrive/c/  
/cygdrive/d/  

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
