# Exploring The Universe! (Part 1)

**Category**: Category

479 points | 7 solves

----

## Challenge Description

Everyone knows about the big universe that surrounds us, where all of us use daily to retrieve latest information and hottest news from all over the world! It is of course, no other than, the one and only, Internet!

To access files and information on the Internet, we all use a protocol called Internet Protocol (IP). This facilitates locating resources throughout the entire network and finds the best way to the destination.

Come on, its 2021! We are moving everything to the decentralised and distributed, away from all those central organisations! There are so many newer and very revolutionary protocols developed to share information on the web other than common protocols such as HTTPS and FTP.

Now enough of introductions.

Agent Myat is working on a university project to explore the massive decentralised universe and has some juicy information hidden there!
However, he is being very tight-lipped about it and we only managed to get these information from him:

`QmZ15yK5GE7gKzKSNC2yj9nLWwNH7sbgyyFcE8pSvzSLMQ`

`k51qzi5uqu5dgrhngvuucsfv4mqmg8btaxhc8zg20ojwfpyl9gcb08ek4381it`

Locate his hidden project and unveal his findings to everyone!

----

## Solution

Reading the description, we can see that we have to look for some decentralised protocol. After doing some searching, we find such a protocol: [IPFS](https://en.wikipedia.org/wiki/InterPlanetary_File_System). The provided string `Qm...` is a hash used by this system, and we can use that to access [Agent Myat's project](https://ipfs.io/ipfs/QmZ15yK5GE7gKzKSNC2yj9nLWwNH7sbgyyFcE8pSvzSLMQ)!

![Screenshot of Agent Myat's web page](https://user-images.githubusercontent.com/40383042/147538504-03cc12f1-b107-4ae0-a90a-a403693e2370.png)

We can then find the flag on the blog page, in the first blog post:

![Screenshot of first blog post, containing the flag](https://user-images.githubusercontent.com/40383042/147538566-131633a6-51da-4db9-a036-9dfad85b29f5.png)

### Flag

```text
IRS{1nT3rP1anet4rY_F1L3_sYs8emz}
```

[Part 2 >](../../Forensics/Exploring%20The%20Universe%21%20%28Part%202%29)
