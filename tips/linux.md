# Tips

## pdf

reduce pdf size : ghostscript

`gs -sDEVICE=pdfwrite -dPDFSETTINGS=/ebook -q -o output.pdf input.pdf`

## renaming files

rename file + autonumber

`$ rename  's/.+/our $i; sprintf("MyGallery_%03d.jpg", 1+$i++)/e' *`
