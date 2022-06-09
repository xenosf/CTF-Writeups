# 🧑‍🎓 Sniffed Traffic

**Category**: Forensics

100 points | 187 solves

----

## Challenge Description

We inspected our logs and found someone downloading a file from a machine **within the same network**.

Can you help find out what the contents of the file are?

For beginners:

* https://www.javatpoint.com/wireshark

## Attached files

* forensics.pcapng

----

## Solution

We can use Wireshark to open `forensics.pcapng`.

Since we know that the file we want to find was downloaded from a machine within the same network, we can narrow it down and look for packets with a source and destination IP address that looks like `192.168.x.x` or some other [private IP range](https://datatracker.ietf.org/doc/html/rfc1918#section-3) (such as `172.16.x.x` or `10.x.x.x`), which means that the device is on the local network.

To do this, we can use this Wireshark filter:

```text
ip.src == 192.168.1.1/16 && ip.dst == 192.168.1.1/16
```

<!--screenshot-->

This massively narrows down the possible packets to search for. After briefly looking through the packets, we can see that a file called `thingamajig.zip` was transferred via HTTP.

We can extract this file and look at it by going to `File > Export Objects > HTTP`, then selecting the file and saving it.

When we try to unzip `thingamajig.zip`, we realise it has a password. We can find the password in the sniffed traffic as well. We can try searching the packet bytes in Wireshark for the word "password":

<!--screenshot-->

We can follow this packet's stream to see the full exchange:

<!--screenshot-->

```text
49949ec89a41ed9bdd18c4ce74f37ae4
```

We can then unzip `thingamajig.zip` using the password we found.

Upon unzipping it, we get a file called `stuff`. To see what it contains, we can `binwalk` it with the `-e` flag to extract its contents:

```sh
$ binwalk -e /Users/atlas/Development/CTF/seetf/forensics_sniffed_traffic/stuff

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
1000          0x3E8           Zip archive data, encrypted at least v1.0 to extract, compressed size: 67, uncompressed size: 55, name: flag.txt
1227          0x4CB           End of Zip archive, footer length: 22
```

Inside `stuff`, there is a zip archive containing `flag.txt`.

Looking at the extracted contents, we can see another password-protected zip archive:

```sh
$ cd _stuff.extracted
$ ls
3E8.zip  flag.txt
$ unzip 3E8.zip
Archive:  3E8.zip
[3E8.zip] flag.txt password:
```

(The `flag.txt` file here is empty, since its contents can't be extracted by `binwalk` properly without the password.)

There's no other useful information in the Wireshark capture that could tell us the second password, so we have to try cracking the password.

We can use `john` to crack the password using the wordlist `rockyou.txt`:

```sh
$ zip2john 3E8.zip >> zip.txt
ver 1.0 efh 5455 efh 7875 3E8.zip/flag.txt PKZIP Encr: 2b chk, TS_chk, cmplen=67, decmplen=55, crc=77D7507A ts=42F2 cs=42f2 type=0

$ john --wordlist=/usr/share/wordlists/rockyou.txt zip.txt
Using default input encoding: UTF-8
Loaded 1 password hash (PKZIP [32/64])
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
john             (3E8.zip/flag.txt)     
1g 0:00:00:00 DONE (2022-06-04 13:44) 16.66g/s 136533p/s 136533c/s 136533C/s 123456..whitetiger
Use the "--show" option to display all of the cracked passwords reliably
Session completed.
```

We now know that the password is `john`. We can now enter the password and get the flag:

```sh
$ unzip 3E8.zip
Archive:  3E8.zip
[3E8.zip] flag.txt password:
 extracting: flag.txt                

$ cat flag.txt       
SEE{w1r35haRk_d0dod0_4c87be4cd5e37eb1e9a676e110fe59e3}
```

### Flag

```text
SEE{w1r35haRk_d0dod0_4c87be4cd5e37eb1e9a676e110fe59e3}
```
