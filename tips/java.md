# Java

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

## Batch file to run maven for a specific jdk  
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

## Display current git branch name in windows command prompt

[stackoverflow](https://stackoverflow.com/questions/36047706/show-current-git-branch-name-in-windows-command-prompt)
[AutoRun](https://ss64.com/nt/syntax-autoexec.html) 
[Doskey](https://superuser.com/questions/118655/auto-execute-command-after-going-to-a-folder-with-the-cd-command)
[Prompt](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/prompt)

C:\home\bin\maven-bin\cd-git.bat :  
```
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

`> regtool -w -s set '/HKCU/Software/Microsoft/Command Processor/AutoRun' 'if exist C:\home\bin\maven-bin\cd-git.bat doskey cd=C:\home\bin\maven-bin\cd-git.bat $*' ` 

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

## Regex Normalizer
```
System.out.println(Normalizer.normalize(CEt ça sera sa moitié, Normalizer.Form.NFD));
CEt ça sera sa moitié
System.out.println(Normalizer.normalize(CEt ça sera sa moitié, Normalizer.Form.NFD).replaceAll([^\\p{ASCII}], )); // \\p{ASCII} : match ascii
CEt ca sera sa moitie
System.out.println(Normalizer.normalize(CEt ça sera sa moitié, Normalizer.Form.NFD).replaceAll([\\p{M}],)); // \\p{M} : match accents
CEt ca sera sa moitie
```

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

## Clob and charset
```
myClob = new javax.sql.rowset.serial.SerialClob(myString.getBytes("ISO-8859-1").toCharArray());
myClob = new javax.sql.rowset.serial.SerialClob(myString.getBytes("UTR-8").toCharArray());
```

---
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

## Maven

Encrypt password :  
`$ mvn --encrypt-password newpass`  > settings.xml 


---

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

## Eclipse  

Eclipse: Java was started but returned error code=13  
Remove in environement PATH : "C:\Program Files (x86)\Common Files\Oracle\Java\javapath"  

[Debug mode - eclipse-weblogic](https://stackoverflow.com/questions/26104666/how-to-debug-java-web-application-in-eclips-with-weblogic-server)  

startWeblogic.cmd :  
`set JAVA_OPTIONS=-Xdebug -Xnoagent -Xrunjdwp:transport=dt_socket,address=8453,server=y,suspend=n ` 
Eclipse (mode debug) :  
Run -> Debug Configurations -> Remote Java Application  
	Host : localhost  
	Port : 8453  
	-> Click Debug   


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
