# Astronaut Document

**Category**: Forensics

230 points

----

## Challenge Description
This document came from outer space, it seems a bit broken, I wonder if someone can fix it.

The flag is in the flag format: STC{...}

**Author:** LegPains

# Attached files
* astronaut_document.txt

----

## Solution
Looking at the start of the document, we can see that it is a PDF of some sort:
```
=pod%PDF-1.4
%�쏢
%%Invocation: gs -sDEVICE=pdfwrite -dCompatibilityLevel=1.4 -dPDFSETTINGS=/printer -dNOPAUSE -dQUIET -dBATCH -sOutputFile=? ?
✂️--- SNIP ---✂️
```

However, something is clearly wrong with it. After changing the extension to .pdf, it shows a blank document.

To analyse this PDF, we can use Didier Stevens' [pdf-parser.py](https://blog.didierstevens.com/programs/pdf-tools/):

```
$ pdf-parser.py astronaut_document.txt
```

Looking at the output, we can see that object 5 contains a stream of compressed data:
```
✂️--- SNIP ---✂️

obj 5 0
 Type: 
 Referencing: 6 0 R
 Contains stream

  <<
    /Length 6 0 R
    /Filter /FlateDecode
  >>

✂️--- SNIP ---✂️
```

We can inflate the 5th object using the same tool.

```
$ pdf-parser.py astronaut_document.txt -o 5 -f
obj 5 0
 Type: 
 Referencing: 6 0 R
 Contains stream

  <<
    /Length 6 0 R
    /Filter /FlateDecode
  >>

 'q 0.1 0 0 0.1 0 0 cm\n/R8 cs\n0 0 0 scn\nq\n10 0 0 10 0 0 cm BT\n/R9 36 Tf\n1 0 0 1 80 285 Tm\n(STC{PDF_Is_4_veRY_Fl3XIBL3_fORmaT})Tj\nET\nQ\nQ\n'
```

The `-o` specifies the object, and `-f` ("filter") applies a filter, which for now only supports zlib decompression (hence there's no need to specify anything).

From the output, we can see that the flag is hidden within the compressed object.

### Flag:
```
STC{PDF_Is_4_veRY_Fl3XIBL3_fORmaT}
```
