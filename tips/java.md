<!-- TOC start (generated with https://github.com/derlin/bitdowntoc) -->

- [Java](#java)
   * [List all available versions of OpenJDK on Linux Mint.](#list-all-available-versions-of-openjdk-on-linux-mint)
   * [Display current git branch name in windows command prompt](#display-current-git-branch-name-in-windows-command-prompt)
   * [Marshaller example](#marshaller-example)
   * [Regex Normalizer](#regex-normalizer)
   * [xpath](#xpath)
   * [PDF : workaround for html2pdf to download pdf : Base64  ](#pdf-workaround-for-html2pdf-to-download-pdf-base64)
   * [Duration  ](#duration)
   * [Clob and charset](#clob-and-charset)
   * [Log4j](#log4j)
   * [Maven](#maven)
      + [Encrypt password :  ](#encrypt-password-)
      + [Script to run maven for a specific jdk  ](#script-to-run-maven-for-a-specific-jdk)
   * [Jetty](#jetty)
   * [Eclipse  ](#eclipse)
   * [Tomcat  ](#tomcat)

<!-- TOC end -->

<!-- TOC --><a name="java"></a>
# Java

<!-- TOC --><a name="list-all-available-versions-of-openjdk-on-linux-mint"></a>
## List all available versions of OpenJDK on Linux Mint.

```shell
$ sudo apt update
$ apt-cache search openjdk
```
To install the default OpenJDK :  
`$ sudo apt install default-jdk`  

Or select a specific version :  
`$ sudo apt install openjdk-17-jdk`  

List of Java versions installed :  
`$ sudo update-java-alternatives --list`  

Set the default version :  
`$ sudo update-alternatives --config java`  

Check the default version:sam jui 27 16:13:02 CEST 2024   
`$ java --version`  


<!-- TOC --><a name="display-current-git-branch-name-in-windows-command-prompt"></a>
## Display current git branch name in windows command prompt

[stackoverflow](https://stackoverflow.com/questions/36047706/show-current-git-branch-name-in-windows-command-prompt)
[AutoRun](https://ss64.com/nt/syntax-autoexec.html) 
[Doskey](https://superuser.com/questions/118655/auto-execute-command-after-going-to-a-folder-with-the-cd-command)
[Prompt](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/prompt)

C:\home\bin\maven-bin\cd-git.bat :  
```cmd
@echo off
CD %*
where git >nul 2>&1
if %errorlevel% neq 0 (
	goto :eof
)
for /f "usebackq tokens=* delims=" %%g in (`git rev-parse --is-inside-work-tree ^>nul 2^>^&1 ^&^& git branch --show-current`) do (
	set branchname=%%g
)
if "%branchname%"=="" (
	prompt $p$g
) else (
	prompt $p $c$e[36m%branchname%$e[0m$f$_$$$s
)
set branchname=
```

regtool  
-w : access 64 bit registry  
-s : set type to REG_SZ (string)  
`> regtool -w -s set '/HKCU/Software/Microsoft/Command Processor/AutoRun' 'if exist C:\home\bin\maven-bin\cd-git.bat doskey cd=C:\home\bin\maven-bin\cd-git.bat $*' ` 

<!-- TOC --><a name="marshaller-example"></a>
## [Marshaller example](https://howtodoinjava.com/jaxb/marshaller-example/)
 "Marshaller Callback Methods"  
 "You can customize the marshalling operation by inside JAXB annotated class e.g. Employee.java. "  
 "You need to define two methods which will listen before and after the marshaller process that class. "  
 "In these methods, you can perform actions such as setting extra fields"  
```java 
// Invoked by Marshaller after it has created an instance of this object.
boolean beforeMarshal(Marshaller marshaller) {
	System.out.println("Before Marshaller Callback");
	return true;
}

// Invoked by Marshaller after it has marshalled all properties of this object.
void afterMarshal(Marshaller marshaller) {
	System.out.println("After Marshaller Callback");
}
```

<!-- TOC --><a name="regex-normalizer"></a>
## Regex Normalizer
```
System.out.println(Normalizer.normalize(CEt ça sera sa moitié, Normalizer.Form.NFD));
CEt ça sera sa moitié
System.out.println(Normalizer.normalize(CEt ça sera sa moitié, Normalizer.Form.NFD).replaceAll([^\\p{ASCII}], )); // \\p{ASCII} : match ascii
CEt ca sera sa moitie
System.out.println(Normalizer.normalize(CEt ça sera sa moitié, Normalizer.Form.NFD).replaceAll([\\p{M}],)); // \\p{M} : match accents
CEt ca sera sa moitie
```

<!-- TOC --><a name="xpath"></a>
## xpath

xpath to get StartDate and EndDate for all declaration types :  
`//*[local-name()='Declarer']//*[local-name()='StartDate'] `  
whatever intermediate element : AccountingYear|ReportingPeriod  

```

	Document doc = DocumentBuilderFactory.newInstance().newDocumentBuilder().parse(new File(filepath);
	XPath xpath = XPathFactory.newInstance().newXPath();

	// NodeList version
	NodeList nodes = (NodeList) xpath.compile(//*[local-name() = 'AssessmentYear']).evaluate(doc,XPathConstants.NODESET);
	for (int i = 0; i < nodes.getLength(); i++) {
		Node node = nodes.item(i);
		assessmentYear = node.getTextContent();
	}

	// SingleNode version
	Node node = (Node) xpath.compile(//*[local-name() = 'AssessmentYear']).evaluate(doc, XPathConstants.NODE);
	assessmentYear = node.getTextContent();
```

<!-- TOC --><a name="pdf-workaround-for-html2pdf-to-download-pdf-base64"></a>
## PDF : workaround for html2pdf to download pdf : Base64  

http://localhost:8080/api/pdf/get/html/base64/
```
src/main/java/sandbox/rest/resource/PdfResource.java :
	@POST
	@Path("get/html/base64/{filename}")
	@Produces(MediaType.TEXT_PLAIN + ";charset=utf-8")
	public Response getPdfBase64FromHtml(String html, @PathParam("filename") String filename)
	[...]
	return Response.ok(
		Base64.getEncoder().encodeToString(baos.toByteArray()).toString(), 
		MediaType.TEXT_PLAIN)
		.build();
```

src/main/webapp/testHtml2pdf.html :
```
[...]
$.ajax(settings)
	.done(function (response) {
		var anchor = document.createElement("a"); //Create anchor <a>
		anchor.href = "data:application/pdf;base64," + response; // Pdf in Base64
		anchor.download = filename;
		anchor.click(); // trigger file download			
		anchor.remove();
```

<!-- TOC --><a name="duration"></a>
## Duration  
```
Instant start = Instant.now();
System.out.println("Start :" + DateTimeFormatter.ofLocalizedDateTime(FormatStyle.SHORT).withLocale(Locale.FRENCH).withZone(ZoneOffset.UTC).format(start));

try {
	Thread.sleep(1000);
} catch (InterruptedException e) {
	e.printStackTrace();
}

Instant two = Instant.now();

System.out.println("Stop  :" + DateTimeFormatter.ofLocalizedDateTime(FormatStyle.SHORT).withLocale(Locale.FRENCH).withZone(ZoneOffset.UTC).format(two));
Duration res = Duration.between(one, two);
System.out.println("Duration : " + res.toMinutesPart() + " min " + res.toSecondsPart() + " s");
```

<!-- TOC --><a name="clob-and-charset"></a>
## Clob and charset
```
myClob = new javax.sql.rowset.serial.SerialClob(myString.getBytes("ISO-8859-1").toCharArray());
myClob = new javax.sql.rowset.serial.SerialClob(myString.getBytes("UTR-8").toCharArray());
```

---
<!-- TOC --><a name="log4j"></a>
## Log4j

Filter log... to skip noisy messages  

src/main/resources/log4j.xml :  
```
<log4j:configuration>[...]
	<appender>[...]
	    <layout/>
		# filter type 1
	    	<filter class="org.apache.log4j.filter.ExpressionFilter">
	    		<param name="expression" value="MSG LIKE '.*t read cell.*'" />
            		<param name="acceptOnMatch" value="false"/>
        	</filter>
 	    	# filter type 2 
	    	<filter class="org.apache.log4j.varia.StringMatchFilter">
		    <param name="StringToMatch" value="Can't read cell" />
		    <param name="acceptOnMatch" value="false"/>
        	</filter>
			# filter not a soluce ;-)
			<filter class="org.apache.log4j.varia.DenyAllFilter"/> -->
	</appender>
[...]
```

---

<!-- TOC --><a name="maven"></a>
## Maven

<!-- TOC --><a name="encrypt-password-"></a>
### Encrypt password :  
`$ mvn --encrypt-password newpass`  > settings.xml 

<!-- TOC --><a name="script-to-run-maven-for-a-specific-jdk"></a>
### Script to run maven for a specific jdk  
mvn8.cmd :  
```cmd
@echo off
@REM Remember to add PATH to this file...

echo "       ____.                       ______    "
echo "      |    |____ ___  _______     /  __  \   "
echo "      |    \__  \\  \/ /\__  \    >      <   "
echo "  /\__|    |/ __ \\   /  / __ \_ /   --   \  "
echo "  \________(____  /\_/  (____  / \______  /  "
echo "                \/           \/         \/   "
echo "                                             "

@REM SET M2_HOME=C:\home\bin\apache-maven-3.9.5
SET JAVA_HOME=C:\home\bin\jdk-8
CALL "C:\home\bin\apache-maven-3.9.5\bin\mvn.cmd" %*
```
mvn8.sh : 
```bash
#!/usr/bin/bash
export JAVA_HOME="C:\home\bin\jdk-17"
echo "      ____.                      _____________   "
echo "     |    |____ ___  _______    /_   \______  \  "
echo "     |    \__  \\  \/ /\__  \    |   |   /    /  "
echo " /\__|    |/ __ \\   /  / __ \_  |   |  /    /   "
echo " \________(____  /\_/  (____  /  |___| /____/    "
echo "               \/           \/                   "
mvn $@ 
```


---

<!-- TOC --><a name="jetty"></a>
## Jetty

Remote debug with mvn jetty:run  

`MAVEN_OPTS="-Xdebug -Xnoagent -Djava.compiler=NONE -Xrunjdwp:transport=dt_socket,address=8000,server=y,suspend=n" && mvn jetty:run | tee log/debug.log`  

Embedded jetty : Fix "favicon.ico 404" in browser  

src/main/webapp/favicon.ico

How to kill running jetty on port 8080  
```
> netstat -ano | grep 8080 | grep LISTENING
> taskkill /F /PID 1234
```

<!-- TOC --><a name="eclipse"></a>
## Eclipse  
**Eclipse: Java was started but returned error code=13**
Remove in environement PATH : "C:\Program Files (x86)\Common Files\Oracle\Java\javapath"  

[Debug mode - eclipse-weblogic](https://stackoverflow.com/questions/26104666/how-to-debug-java-web-application-in-eclips-with-weblogic-server)  

startWeblogic.cmd :  
`set JAVA_OPTIONS=-Xdebug -Xnoagent -Xrunjdwp:transport=dt_socket,address=8453,server=y,suspend=n ` 
Eclipse (mode debug) :  
Run -> Debug Configurations -> Remote Java Application  
	Host : localhost  
	Port : 8453  
	-> Click Debug   

**Eclipse 2023-09 : ObjectAid plugin**  
[objectaid-eclipse](https://stackoverflow.com/questions/68589918/objectaid-unhandled-event-loop-exception/70785096#70785096)  

There is a workaround:  

1. Download xstream 1.4.18 jar  

2. Locate the com.objectaid.uml_1.2.4.jar file and open it (with the zip tool of your choice)  

3. Delete the xstream-1.3.1.jar file inside the lib directory.  
Add the xstream-1.4.18.jar to the lib directory.  

4. Open the file META-INF/MANIFEST.MF and replace line lib/xstream-1.3.1.jar with lib/xstream-1.4.18.jar.  

5. Add to your eclipse.ini the following vmargs:   
--add-opens=java.base/java.lang=ALL-UNNAMED  
--add-opens=java.base/java.util=ALL-UNNAMED  
--add-opens=java.base/java.text=ALL-UNNAMED  
--add-opens=java.desktop/java.awt.font=ALL-UNNAMED  

6. Restart Eclipse with -clean to ensure the OSGI cache is purged.  

7. Install the OjbectAid plugin :  
Help > Install New Software... > Add > Archive : Select "objectaid-1.2.4.zip"  
Restart Eclipse  

8. In Project Explorer : Ctrl+N (New) > Select "ObjectAid UML Diagram" > "ObjectAid Class Diagram"   
and call it "uml"or whatever  

9. Drag and Drop java classes into diagram "uml.ucls"  



<!-- TOC --><a name="tomcat"></a>
## Tomcat  

Tomcat 7 does not start anymore... ?  
> Install Tomcat 9  
Source of new problem : conflicting port for shutdown !!!  
D:\tools\DEV\SERVER\apache-tomcat-9.0.30\conf\server.xml :  
```
<Server port="8005" shutdown="SHUTDOWN">
to
<Server port="8006" shutdown="SHUTDOWN">
# idem for Tomcat 7 + set environement
D:\tools\DEV\SERVER\apache-tomcat-7.0.70\bin\startup.bat :
	setlocal
	SET CATALINA_HOME=D:\tools\DEV\SERVER\apache-tomcat-7.0.70
	SET CATALINA_BASE=D:\tools\DEV\SERVER\apache-tomcat-7.0.70
	SET CATALINA_TMPDIR=D:\tools\DEV\SERVER\apache-tomcat-7.0.70\temp
	SET JAVA_HOME==C:\Program Files\Java\jdk1.7.0_80
	SET JRE_HOME=C:\Program Files\Java\jdk1.7.0_80\jre
```
