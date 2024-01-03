# TO TEST

## pandoc
[pandoc](https://pandoc.org/)  
Universal document converter  

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
