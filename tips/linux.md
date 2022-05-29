# Linux Tips

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

`$ sudo cp -r ~/Downloads/wiktionnaire/StarDict/dict-fr /usr/share/stardict/dic/`

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
