# 🧑‍🎓 Regex101

**Category**: Miscellaneous

100 points | 382 solves

----

## Challenge Description

Our team stored a flag on our machine, however, we were hacked by someone, and he generated 2999 flags and hid our original flag in the .txt file. The flag consists of 5 uppercase letters, followed by 5 digits and another 6 uppercase letters. Can you find it for us?

## Attached files

* flags.txt

----

## Solution

`flags.txt` contains 3000 lines with a possible flag. Given the information in the description, we can construct a regular expression that we can use to find the correct flag.

The flag comprises several parts that we can translate into parts of a regular expression:

1. Start of the flag &rarr; `SEETF{`
2. 5 uppercase letters &rarr; `[A-Z]` (any uppercase letter) + `{5}` (5 of them)
3. 5 digits &rarr; `[0-9]` (any digit from 0-9) + `{5}` (5 of them)
4. 6 uppercase letters &rarr; `[A-Z]` (any uppercase letter) + `{6}` (6 of them)
5. End of the flag &rarr; `}`

Putting it together, we get this regular expression:

```regex
SEE{[A-Z]{5}[0-9]{5}[A-Z]{6}}
```

We can then use this regular expression with `grep -E` (to search using a regular expression) to search the file for the correct flag:

```sh
$ grep -E "SEE{[A-Z]{5}[0-9]{5}[A-Z]{6}}" flags.txt
SEE{RGSXG13841KLWIUO}
```

### Flag

```text
SEE{RGSXG13841KLWIUO}
```

## Footnote

A useful website for creating and testing regular expressions is [regexr.com](https://regexr.com/).
