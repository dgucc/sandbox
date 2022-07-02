# Javascript

## Custom event in d3
```
var dispatch = d3.dispatch("statechange");
dispatch.on('statechange', function(e){ console.log(e) })
dispatch.call("statechange", this, 'Hello, world!')
```

## String Remove accents 
```
String.prototype.sansAccent = function(){
    var accent = [
        /[\300-\306]/g, /[\340-\346]/g, // A, a
        /[\310-\313]/g, /[\350-\353]/g, // E, e
        /[\314-\317]/g, /[\354-\357]/g, // I, i
        /[\322-\330]/g, /[\362-\370]/g, // O, o
        /[\331-\334]/g, /[\371-\374]/g, // U, u
        /[\321]/g, /[\361]/g, // N, n
        /[\307]/g, /[\347]/g, // C, c
    ];
    var noaccent = ['A','a','E','e','I','i','O','o','U','u','N','n','C','c'];
     
    var str = this;
    for(var i = 0; i < accent.length; i++){
        str = str.replace(accent[i], noaccent[i]);
    }
     
    return str;
}
var chaine = "À côté d'un verre vide, il y a toujours un mec plein.";
alert( chaine.sansAccent() );
```
## PDf : workaround for html2pdf to download pdf with javascript : Base64
```
http://localhost:8080/api/pdf/get/html/base64/
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

src/main/webapp/testHtml2pdf.html :
[...]
$.ajax(settings)
	.done(function (response) {
		var anchor = document.createElement("a"); //Create anchor <a>
		anchor.href = "data:application/pdf;base64," + response; // Pdf in Base64
		anchor.download = filename;
		anchor.click(); // trigger file download			
		anchor.remove();
```
