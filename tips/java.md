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
