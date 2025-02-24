<!-- TOC start (generated with https://github.com/derlin/bitdowntoc) -->

- [Javascript](#javascript)
   * [Fix charset rendering ](#fix-charset-rendering)
   * [String Remove accents ](#string-remove-accents)
   * [jq : json query ](#jq-json-query)
   * [iconv](#iconv)
   * [PDF : workaround for html2pdf to download pdf with javascript : Base64](#pdf-workaround-for-html2pdf-to-download-pdf-with-javascript-base64)
   * [CompanyNumber cleanup :  ](#companynumber-cleanup-)
   * [d3.js](#d3js)
   * [Misc](#misc)
     + [Programmatically define input:file](#Programmatically-define-input-file)
     + [Template Literals](#template-literals)

<!-- TOC end -->

<!-- TOC --><a name="javascript"></a>
# Javascript

<!-- TOC --><a name="fix-charset-rendering"></a>
## Fix charset rendering 
```
var utf8_to_latin1 = function (s) {
    return unescape(encodeURIComponent(s));
};
var latin1_to_utf8 = function (s) {
    return decodeURIComponent(escape(s));
};
```

<!-- TOC --><a name="string-remove-accents"></a>
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

<!-- TOC --><a name="jq-json-query"></a>
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

File encoding may prevent jq to work properly

<!-- TOC --><a name="iconv"></a>
## iconv

Check file encoding  

`$ file -i file.json` 

> file.json: text/plain; charset=utf-16le

Change file encoding    

`$ iconv -f UTF-16LE -t UTF-8 file.json > file-new.json`  

Or (with TRANSLIT)   

`$ iconv -f UTF-16LE -t UTF-8//TRANSLIT file.json > file-new.json`  

Or (script version)   

```
#!/usr/bin/bash
# Force file encoding to UTF-8
for file in $(ls *.json) 
do
	# get previous encoding by parsing file output
	# input.json: text/plain; charset=utf-16le
	ENC_FROM=$(file -i $file | cut -d';' -f2 | cut -d'=' -f2)	
	
	# //TRANSLIT if supported (cf. iconv version)
	ENC_TO=UTF-8//TRANSLIT
	iconv -f ${ENC_FROM} -t ${ENC_TO} $file > "${file%.json}_new.json"
	
	# Check result
	file -i "${file%.json}_new.json"
done
```

Or (by using vim in "ex" and "silen" mode)    

`$  vim -es '+set fileencoding=utf-8' '+wq!' file.json `  


<!-- TOC --><a name="pdf-workaround-for-html2pdf-to-download-pdf-with-javascript-base64"></a>
## PDF : workaround for html2pdf to download pdf with javascript : Base64

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

## wkhtmltopdf

###How to produce A4 size PDFs
**css:**  
```css
{
   width: 991px;
   height: 1403px;
}
```
**wkhtmltopdf options:**  
`wkhtmltopdf -L 0 -R 0 -T 0 -B 0 -s A4 --disable-smart-shrinking input.html output.pdf`

<!-- TOC --><a name="companynumber-cleanup-"></a>
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

<!-- TOC --><a name="d3js"></a>
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

## Misc

### Programmatically define input:file 

```js
// programmatically add XSD in input filelist
const xsdFileName = "xsd/schema.xsd"
loadURLToInputFiled(xsdFileName);

// Use DataTransfer.files to define input.files
function loadURLToInputFiled(url) {
  getImgURL(url, (fileBlob) => {
	let fileName = xsdFileName;
	let file = new File([fileBlob], fileName, { type: "text/xml", lastModified: new Date().getTime() }, 'utf-8');
	let dataTransfer = new DataTransfer();
	dataTransfer.items.add(file);
	document.querySelector('#get_the_file').files = dataTransfer.files;
	document.getElementById("get_the_file").dispatchEvent(new Event("change"));
  })
}
// xmlHTTP return blob respond
function getImgURL(url, callback) {
  var xhr = new XMLHttpRequest();
  xhr.onload = function () {
	callback(xhr.response);
  };
  xhr.open('GET', url);
  xhr.responseType = 'blob';
  xhr.send();
}
```

### Teamplate Literals

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals)  

```javascript
let msg="hello world!";
let templateLiterals = `
<div class="hello">
   ${msg}
</div>`;
console.log(templateLiterals);
```
 
