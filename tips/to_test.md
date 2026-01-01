# TO TEST
- [pandoc](#pandoc)
- [tika](#tika)
- [scribe](#scribe)
- [EA](#enterprise-architect)
- [UML](#uml)
  - [ObjectAid Eclipse 2023-09](#objectaid-eclipse-2023-09)
  - [StarUML](#staruml)
- [DBeaver](#dbeaver)
- [Misc](#misc)

## pandoc
[pandoc](https://pandoc.org/)  
Universal document converter  
`$ sudo apt-get install pandoc`  

### [Examples](https://pandoc.org/demos.html)
- Text to epub  
`$ pandoc --toc --metadata title="Title" -s input.txt -o output.epub`
- Text to markdown  
`$ pandoc --toc --metadata title="Title" -s input.txt -t markdown -o output.md`  
- Web page to markdown  
`$ pandoc -s -r html http://www.gnu.org/software/make/ -o example12.text`  
- Markdown to pdf
  $ pandoc README.md --pdf-engine=xelatex -o output.pdf`

  
## tika
[tika](https://tika.apache.org/)  
The Apache Tika™ toolkit detects and extracts metadata and text from over a thousand different file types (such as PPT, XLS, and PDF). All of these file types can be parsed through a single interface, making Tika useful for search engine indexing, content analysis, translation, and much more.  

[tutorialspoint](https://www.tutorialspoint.com/tika/index.htm)  

## scribe

[scribe : Un outil de transcription gratuit et open source](https://www.blogdumoderateur.com/tools/redirection/?tool_id=27899&tool_name=scribe)  

[github](https://gitlab.cemea.org/mallette/scribe)  

## Enterprise Architect

[Using Wine](https://sparxsystems.com/enterprise_architect_user_guide/14.0/product_information/enterprise_architect_linux.html)  

  Install Wine and Winetricks

```bash
sudo dpkg --add-architecture i386
sudo add-apt-repository ppa:wine/wine-builds
sudo apt-get update
sudo apt-get install --install-recommends winehq-devel
sudo apt-get install winetricks
```
  
  Recommended installing the Carlito font

```bash
sudo apt-get install fonts-crosextra-carlito
```

  Install the MSXML and MDAC components by running these commands:

```bash
winetricks msxml3
winetricks msxml4
winetricks mdac28
```
  If prompted to do so, download the msxml.msi file and place it in the directory:
```bash  
~/.cache/winetricks/msxml3.
```

Note:
When Wine updates are applied to your system, often they interfere with the mdac28 module, preventing it from running.
To avoid any issues, it is good practice after installing Wine updates, to run this command:
```bash
winetricks --force mdac28
```

  Install Enterprise Architect by running one of these commands, which relate to the registered and trial installations respectively:
```bash
wine msiexec /i easetupfull.msi
```
## UML

[Prise en main d'outils UML](https://github.com/iblasquez/tuto_ModelisationUML)


### ObjectAid Eclipse 2023-09

[stackoverflow ](https://stackoverflow.com/questions/68589918/objectaid-unhandled-event-loop-exception/70785096#70785096)
It seems that ObjectAid is no longer maintained. I got this error running on JDK17.

There is a workaround:

1. Download  
  [objectaid-1.1.14.zip](http://web.archive.org/web/20190113065017/http://www.objectaid.com/update/1.1/objectaid-1.1.14.zip)  
  [xstream 1.4.18 jar](https://repo1.maven.org/maven2/com/thoughtworks/xstream/xstream/1.4.18/xstream-1.4.18.jar)   
1. Locate the `com.objectaid.uml_1.2.4.jar` file and open it (with the zip tool of your choice)  
1. Delete the xstream-1.3.1.jar file inside the lib directory. Add the xstream-1.4.18.jar to the lib directory.   
1. Open the file `META-INF/MANIFEST.MF` and replace line `lib/xstream-1.3.1.jar` with `lib/xstream-1.4.18.jar`.
1. Repack the folders "features" and "plugins" into `objectaid-1.1.14.zip`
1. Add to your `eclipse.ini` the following vmargs:
```
--add-opens=java.base/java.lang=ALL-UNNAMED
--add-opens=java.base/java.util=ALL-UNNAMED
--add-opens=java.base/java.text=ALL-UNNAMED
--add-opens=java.desktop/java.awt.font=ALL-UNNAMED
```  
1. Restart Eclipse with `-clean` to ensure the OSGI cache is purged.  
1. Install the OjbectAid plugin :  
   Help > Install New Software... > Add > Archive : Select "objectaid-1.2.4.zip"  
   Restart Eclipse  
1. In Project Explorer : Ctrl+N (New) > Select "ObjectAid UML Diagram" > "ObjectAid Class Diagram" and call it "uml.ucls"
1. Drag and Drop java classes into diagram  

### StarUML

*How to get full version of StarUML*

cf. this [hack](https://gist.github.com/trandaison/40b1d83618ae8e3d2da59df8c395093a?permalink_comment_id=4805024#gistcomment-4805024)


An update from my initial post.
This worked for me on v5.0.1 v5.1.0 (Linux).

All commands were run as root
You may need to run commands as root if your StarUML directory is not owned by your user

Probably should have StarUML closed during this process
I recommend backing up your StarUML directory

1. Install asar via npm
```sh
npm i asar -g
```

2. Extract source

```sh
cd <path_to_staruml>/resources/ # mine was /opt/StarUML/resources/
asar extract app.asar app
```

3. Edit src/engine/license-manager.js
change this
```javascript
checkLicenseValidity () {
  if (packageJSON.config.setappBuild) {
    setStatus(this, true)
  } else {
    this.validate().then(() => {
      setStatus(this, true)
    }, () => {
      setStatus(this, false)
      UnregisteredDialog.showDialog()
    })
  }
}
```
to this
```javascript
checkLicenseValidity () {
    setStatus(this, true)
}
```
and this
```javascript
register (licenseKey) {
  return new Promise((resolve, reject) => {
    $.post(app.config.validation_url, {licenseKey: licenseKey})
      .done(data => {
        if (data.product === packageJSON.config.product_id) {
          var file = path.join(app.getUserPath(), '/license.key')
          fs.writeFileSync(file, JSON.stringify(data, 2))
          licenseInfo = data
          setStatus(this, true)
          resolve(data)
        } else {
          setStatus(this, false)
          reject('unmatched') /* License is for old version */
        }
      })
      .fail(err => {
        setStatus(this, false)
        if (err.status === 499) { /* License key not exists */
          reject('invalid')
        } else {
          reject()
        }
      })
  })
}
```
to this
```javascript
register (licenseKey) {
  return new Promise(() => { setStatus(this, false) })
}
```

4. Repackage app
```sh
asar pack app app.asar
```
Next time you run StarUML, it should behave as a licensed version.
Source (Nonfunctional as of this post) Source on Wayback Original Post
```javascript
checkLicenseValidity () {
  if (packageJSON.config.setappBuild) {
    setStatus(this, true)
  } else {
    this.validate().then(() => {
      setStatus(this, true)
    }, () => {
      setStatus(this, false)
      UnregisteredDialog.showDialog()
    })
  }
}
```
to
```javascript
checkLicenseValidity () {
  if (packageJSON.config.setappBuild) {
    setStatus(this, true)
  } else {
    this.validate().then(() => {
      setStatus(this, true)
    }, () => {
      setStatus(this, true)
    })
  }
}
```
This work for 6.0.1, but i can't export the diagram. I recommend using 5.1.0

## dbeaver
**How to recover db previous passwords**  
DBeaver v23.2.5.202311191730  

**Get Key from github + python :**  
[go to github](https://github.com/dbeaver/dbeaver/blob/d69a75e63bf0a00e37f6b4ab9c9aa4fcaa0ded23/plugins/org.jkiss.dbeaver.model/src/org/jkiss/dbeaver/model/impl/app/DefaultSecureStorage.java#L32
)  
```
public class DefaultSecureStorage implements DBASecureStorage {
    private static final byte[] LOCAL_KEY_CACHE = new byte[] { -70, -69, 74, -97, 119, 74, -72, 83, -55, 108, 45, 101, 61, -2, 84, 74 };
[...]
```

**Decrypt** 
```python
$ python
>>> import struct
>>> struct.pack('<16b', -70, -69, 74, -97, 119, 74, -72, 83, -55, 108, 45, 101, 61, -2, 84, 74).hex()
'babb4a9f774ab853c96c2d653dfe544a'
```

**Decrypt previous stored passwords**  
```bash
openssl aes-128-cbc -d \
  -K babb4a9f774ab853c96c2d653dfe544a \
  -iv 00000000000000000000000000000000 \
  -in " %APPDATA%\DBeaverData\workspace6\General\.dbeaver\.credentials-config.json.bak" | \
  dd bs=1 skip=16 2>/dev/null | jq
```  

```json
 {
  "db2-18bdd50d52b-3539a17d1da04651": {
    "#connection": {
      "user": "username",
      "password": "***"
    }
  },
  "db2-18bf61fac40-6a96b1f60ae10d76": {
    "#connection": {
      "user": "username",
      "password": "***"
    }
  },
  "db2-18c01312201-65b47a439b371c6c": {
    "#connection": {
      "user": "username",
      "password": "***"
    }
  },
  "db2-18c02137976-55f5c4f7710a5855": {
    "#connection": {
      "user": "username",
      "password": "***"
    }
  },
  "db2-18ee61c48e0-3865c77a810af7cc": {
    "#connection": {
      "user": "username",
      "password": "***"
    }
  }
}
```

---
## How to Clone Windows 11/10 to a New SSD/HDD Without Any Software

[youtube](https://www.youtube.com/watch?v=CDLyNJGzkWE)


### Create Backup file - dism

Shift + Restart

```
(as admin) > dism /capture-image /imagefile:D:\backup.wim /capturedir:C:\ /name:"WinBackup" /compress:max /checkintegrity
```

Open notepad to check Windows is on C:\

### Prepare new disk

Reboot Windows  

Open Disk Manager  
Choose GPT as partition style in Diaglog (for new HDD)  


### Create EFI Partition on S:  

```
(as admin) > diskpart
list disk
sel disk 0
create partition efi size=512
format fs=fat32 quick label="System Boot"
assign letter S
```

Create new partition with Disk Manager (NTFS as W:)  

### Apply backup file to W:
```
(as admin) > D:
dism /apply-image /imagefile:backup.wim /index:1 /applydir:W:\
```

Activate new boot partition
```
(as admin) > bcdboot W:\Windows /f uefi /s S:\
```

Restart > BIOS : select new HDD


---

## Misc

Cemu on Linux cf. Lutris...  

WiiU-USB on Linux : cf. Wine-stage  

PCSx2 : pro controller Linux  

Yuzy : Switch emulator  cf. youtube Daniel Schmid
https://www.xci-nsp.com
yuzu-emu.org  
Galiak Game


Remote Control for AndroidTV
> codes 
10381  
67664537  
51180  
