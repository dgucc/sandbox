# XML

## XMLLINT

Format XML :  
```xml
cat data.xml | xmllint --format -
```
Parse XML :  
Use local-name() trick to ignore namespace prefixes
```xml
cat data.xml | xmllint --xpath '//*[local-name()="name"]/text()' -
```
## XSD Viewer

[xsd-schema-viewer](https://github.com/peterraf/online-xsd-viewer)  

[off-line?](https://github.com/dgucc/sandbox/blob/main/tips/xsdviewer.html)  

## WSDL-Viewer

[Download wsdl-viewer.xsl](https://github.com/qvantel/wsdl-viewer/blob/master/wsdl-viewer.xsl)

Include xsl into wsdl.xml  

```xml
<?xml version='1.0' encoding='UTF-8'?>
<?xml-stylesheet type="text/xsl" href="wsdl-viewer.xsl" ?>
```
Correct the xsl 
```xml
<!--
  <div
			class="porttitle" id="{concat($PORT-TITLE-PREFIX, generate-id($port-type))}">
			<xsl:if test="position() != 1">
				<xsl:value-of select="$collapsed-img" disable-output-escaping="yes" />
			</xsl:if>
    <xsl:if
				test="position() = 1">
				<xsl:value-of select="$expanded-img" disable-output-escaping="yes" />
			</xsl:if>
		Port: <span class="portbold">
				<xsl:value-of select="@name" />
			</span>
		</div>
 -->
```
Open xml file with your browser :  

![image](https://github.com/user-attachments/assets/4704945d-eaa4-4d4d-9dc1-ac35082b1183)
