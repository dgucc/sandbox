# Linux Tips

## date format

`$ echo "LC_TIME=nl_BE.UTF-8" >> $HOME/.profile`

## pdf

reduce pdf size : ghostscript

`gs -sDEVICE=pdfwrite -dPDFSETTINGS=/ebook -q -o output.pdf input.pdf`

## renaming files

rename file + autonumber

`$ rename  's/.+/our $i; sprintf("MyGallery_%03d.jpg", 1+$i++)/e' *`

## ssh

Activate ssh
```
$ sudo apt-get install openssh-server
$ systemctl status sshd
$ systemctl enable sshd
```

## StarDict + wiktionnaire

- Download and install StarDict (https://sourceforge.net/projects/stardict-4/files/3.0.6.2/stardict_3.0.6.2-1_all.deb/download)

stardict_3.0.6.2-1_all.deb

- [Download dictionaries] (https://github.com/BoboTiG/ebook-reader-dict/releases/tag/fr)

- Unzip and copy dict-fr into stardict folder

`$ sudo cp -r dict-fr /usr/share/stardict/dic/`

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

## ffmpeg

- Cut Sample Video 

38:43 : start
04:26 : duration
43:09 : end

`$ ffmpeg -ss 00:38:43 -t 00:04:26 -i "input.avi" -acodec copy -vcodec copy sample.avi`

- mp4 to gif

`$ ffmpeg -i input.mp4 -vf "fps=10,scale=320:-1:flags=lanczos" -c:v pam -f image2pipe - | convert -delay 10 - -loop 0 -layers optimize output.gif]` 


## youtube-dl

`$ sudo apt-get install youtube-dl`
`$ sudo apt-get install python3-pip python-dev`

## HP Printer

`$ sudo hp-setup -i`

---

## GPG error

`$ sudo apt-get update`
"GPG error: http://dl.google.com stable Release: The following signatures couldn't be verified because the public key is not available: NO_PUBKEY 78BD65473CB3BD13"

`wget -q -O - https://dl.google.com/linux/linux_signing_key.pub | sudo apt-key add -
