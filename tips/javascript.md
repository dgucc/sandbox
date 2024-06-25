# Javascript

## Fix charset rendering 
```
var utf8_to_latin1 = function (s) {
    return unescape(encodeURIComponent(s));
};
var latin1_to_utf8 = function (s) {
    return decodeURIComponent(escape(s));
};
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

## jq : json query 

Filter output for selected fields :  
`$ jq -r '[ .[] | {id: .id, field1: .field1} ]' file.json `  

Filter array by range :  
`$ jq -r '[ .[293:296] | .[] | {id: .id, field1: .field1} ]' file.json`  

Export to csv (problem if order of columns matters) :  

`$ jq -r -f filter.jq file.json > out.csv`  

```
 filter.jq :
def tocsv:
    (map(keys)
        |add
        |unique
    ) as $cols
    |map(. as $row
        |$cols
        |map($row[.]|tostring)
    ) as $rows
    |$cols,$rows[]
    | @csv;
tocsv
```
or (one-line)  
`$ jq 'map(. | .id, .field1, .field2 | tostring) | @csv' input.json > output.csv`  

`$ jq -r '.[] | [.field1, .field2, .field3] | @csv' input.json > output.csv`  

some arithmetic on fields :  
`$ jq -r '.[] | [.field1, .field2, (.fieldX | tonumber)+(.fieldY | tonumber)] | @csv ' input.json > output.csv`  

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

## CompanyNumber cleanup :  
```
function cleanupCbe(cbe) {
	let result = (cbe=="" || cbe==undefined) 
			? "" 
			: new Intl.NumberFormat('fr-BE',{minimumIntegerDigits:10, useGrouping:false}).format(cbe.replace(/\D/g,''));
	return result;
}
```
---

## d3.js

d3 Directed Forece Layout - Glossary :  
```
The <defs> element is used to define SVG elements without rendering them. 
	Combined with a ‘g’ tag, it can be used to create a template for 
	an intricate graphic that contains many elements.
The <marker> element defines the graphic that is to be used for 
 	drawing arrowheads or polymarkers on a given <path>, <line>, <polyline> or <polygon> element.
The “charge” force — 
	like electrostatic charge, when negative will push nodes away, 
	(default strength is -30) the charge force is global: 
	every node affects every other node, even if they are on disconnected subgraphs 
	(used with many-body().
The “center” force — 
	centering force, centers all nodes at the given position ?x,y?.
The “link” force — 
	The link force pushes linked nodes together or apart according to the desired link distance.
forceLink: 
	Creates a new link force with the specified links and default parameters. 
	If links are not specified, it defaults to the empty array.
The “many-body” force 
	applies mutually amongst all nodes. It can be used to simulate gravity (attraction) 
	if the strength is a positive, or electrostatic charge (repulsion) 
	if the strength is negative.
alphaTarget: 
	Usually used on interactions such as drag-drop a node, together with restart(), 
	it’s hard to understand — thus hard to explain what is the “alpha” concept, 
	it plays a role in “slowing” the nodes transition, for a “smother” experience 
	when changing the layout by dropping the node somewhere, according to the API docs: 
	alphaTarget sets the current target alpha to the specified number 
	in the range [0,1] and returns this simulation.
tick — 
	The simulator starts automatically, and after the node data iteration the tick event 
	is triggered, on its handler fn the nodes and lines get a calculated x- and y-positions. 
```

Custom event in d3
```
var dispatch = d3.dispatch("statechange");
dispatch.on('statechange', function(e){ console.log(e) })
dispatch.call("statechange", this, 'Hello, world!')
```
