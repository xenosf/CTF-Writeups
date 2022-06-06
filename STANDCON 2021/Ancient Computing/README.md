# Ancient Computing

**Category**: Forensics

100 points

----

## Challenge Description

What is the value contained in the cell D9? Specify your answer up to 14 decimal places.

The flag is the answer to the question enclosed with the flag format: STC{0.12345678901234}

**Author:** icebear
  
## Attached files

* BOOK1.WK1

----

## Solution

Firstly, we have to find out what the heck a .WK1 file is. According to [this website](https://www.file-extension.org/extensions/wk1):

> WK1 is a spreadsheet document file format used by no longer developed software Lotus 1-2-3,which was included in Lotus Smart Suite software productivity suite. WK1 documents were very popular in 1990s when Lotus products were competing with Microsoft office tools.

Dang, that's old. So old, in fact, that Microsoft Excel stopped supporting them after Excel 2007.

This website also says that these files can be opened using OpenOffice. After installing it, indeed, I could open it, and read cell D9.

<img width="1238" alt="Screenshot 2021-07-24 at 00 21 31" src="https://user-images.githubusercontent.com/40383042/126899380-f5c8b2fa-bc61-495f-a5fb-07e46b088d3f.png">

### Flag

```text
STC{0.25241301339338}
```
