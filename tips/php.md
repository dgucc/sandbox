# PHP

## php-5.6.9

https://windows.php.net/downloads/releases/archives/php-5.6.9-Win32-VC11-x64.zip

## SoapClient

Activate extensions 
1. php.ini :  

```php
; Directory in which the loadable extensions (modules) reside.
; http://php.net/extension-dir
; extension_dir = "./"
; On windows:
extension_dir = "ext"
[...]
; The MIBS data available in the PHP distribution must be installed. 
; See http://www.php.net/manual/en/snmp.installation.php 
extension=php_snmp.dll

extension=php_soap.dll
extension=php_sockets.dll
extension=php_sqlite3.dll
extension=php_sybase_ct.dll
extension=php_tidy.dll
extension=php_xmlrpc.dll
extension=php_xsl.dll
```
2. Intall MIBS as indicated  
[Extract net-snmp-5.9.4.zip](https://sourceforge.net/projects/net-snmp/files/net-snmp/5.9.4/net-snmp-5.9.4.zip/download)  
Create a new environment variable MIBDIRS pointing to [extracted-path]\net-snmp-5.9.4

3. Check installation with phpinfo() and look for SOAP section
```php
<?php
phpinfo();
?>
```

4. Test Public API 
```php
<?php
$wsdl = "http://webservices.oorsprong.org/websamples.countryinfo/CountryInfoService.wso?WSDL";
try {
    $client = new SoapClient($wsdl);
    $response = $client->ListOfCountryNamesByName();

    echo "Liste des pays :\n";
    foreach ($response->ListOfCountryNamesByNameResult->tCountryCodeAndName as $country) {
        echo "[" . $country->sISOCode . "] " . $country->sName . "<br>\n";
    }
} catch (SoapFault $fault) {
    echo "Erreur : " . $fault->getMessage();
}
?>
```

5. [TODO] example with a payload in XML


