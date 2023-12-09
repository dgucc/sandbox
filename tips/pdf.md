# pdf tips

## Reduce pdf size with ghostscript   
`$ gs -sDEVICE=pdfwrite -dPDFSETTINGS=/ebook -q -o output.pdf input.pdf`

## pdf to text : install "poppler" to use pdfTotext   
`$ pdftotext input.pdf output.txt` 

## pdf page to thumbnail (jpg,png)

1st pdf page to jpg  
`$ convert example.pdf[0] -quality 90 -resize 50% cover.jpg`  

1st pdf page to jpg with white background  
`$ convert example.pdf[0] -quality 90 -resize 50%  -background white -alpha remove cover.jpg`  

Transparency...  
`$ convert example.pdf[0] -density 175 -colorspace sRGB -resize 50% -quality 95 cover.png`  

Error `attempt to perform an operation not allowed by the security policy 'PDF' ...`  
Edit /etc/ImageMagick-6/policy.xml and comment these :  

```xml
<!-- disable ghostscript format types -->
<!--<policy domain="coder" rights="none" pattern="PS" /> -->
<!--<policy domain="coder" rights="none" pattern="PS2" /> -->
<!--<policy domain="coder" rights="none" pattern="PS3" /> -->
<!--<policy domain="coder" rights="none" pattern="EPS" /> -->
<!--<policy domain="coder" rights="none" pattern="PDF" /> -->
<!--<policy domain="coder" rights="none" pattern="XPS" /> -->
```

## Enhance jpg scan => pdf  
**noteshrink** [python]: Convert scans of handwritten notes to beautiful, compact PDFs  
```
$ git clone https://github.com/mzucker/noteshrink.git  
# Pre-requisites  
$ sudo apt install imagemagick  
$ pip install numpy  
$ pip install scipy  
$ pip install pillow  
# as root, edit /etc/ImageMagick-6/policy.xml :  
 <policy domain="coder" rights="read | write" pattern="PDF" />  
$ cd test/  
$ python3 noteshrink.py *.jpg  
# result page0000.png page0001.png ... output.pdf  
```

## View and edit pdf's meta-data using exiftool  
`$ sudo apt-get update && sudo apt-get install -y libimage-exiftool-perl`
`$ exiftool document.pdf`  
`$ exiftool -Author -Creator -Title document.pdf`  
`$ exiftool -Author="Mr Smith" -Creator="Mrs Smith" -Title="New Title" document.pdf`  
To install with cygwin, search for 'perl-Image-exiftool'  

## How to crop footer with ghostscript  
```
# .HWMargins [ left bottom right up]
# 1 dot = 1/72 inch
# dot = 1/72 inch     |   inch = 2.54 cm      |   cm
# ----------------    |   ------------------  |   -----------------    
#  1	                |   0.013888888888889   |   0.035277777777778
# 28.3464566929134    |	  0.393700787401575   |   1

$ gs -sDEVICE=pdfwrite -o cropped.pdf \
-sDEVICE=pdfwrite \
-dPDFSETTINGS=/ebook \
-dFirstPage=38 -dLastPage=2431 \
-c "mark /.HWMargins [ 0.0 28.35 0.0 0.0 ] .dicttomark setpagedevice" \
-f input.pdf
```

## How to extract images with pdfimages 
$ pdfimages -f 2 -l 2 -j input.pdf temp/cover # cover : prefix

