# Heads and Tails Part 1

**Category**: Miscellaneous

50 points | 50 solves

----

## Challenge Description

Get the ***Heads and Tails***
then you **win**!

## Attached files

* heads_and_tails_part1.jpg

----

## Solution

Using `strings`, we can find the 2 parts ('head' and 'tail') of the flag:

```text
strings heads_and_tails_part1.jpg | less
```

Output:

```text
JFIF
IRS{Got_Head_
...
...
Got_Tail_YAY_Y0u_W1nnEd_tH1S_G4m3!}     
```

Combining the two halves, we get the flag.

### Flag

```text
IRS{Got_Head_Got_Tail_YAY_Y0u_W1nnEd_tH1S_G4m3!}   
```
