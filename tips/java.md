# Java

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
