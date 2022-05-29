# Tips

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
