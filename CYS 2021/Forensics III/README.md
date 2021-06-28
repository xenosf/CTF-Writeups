# Forensics III

## Challenge Description
A memory dump of a Windows machine is provided in the home directory of the root user. You have to use Volatility to analyze the memory dump and answer the following questions:

1. The Attackers have exploited the server remotely and spawned multiple processes to take over the machine. What is the filename of the server which was compromised?
2. The exploited process is exposing a port which the attackers are using to connect to the bind shell. Which port number is it?
3. The attackers seem to have migrated their shells into a core process. Can you identify the process by name (Provide the full name of the binary for that process listed in process list)?
4. The exploited process has launched multiple firefox processes. Most probably either it is visiting a web page or downloading payload using the browser. What is the domain name of the accessed portal? (Hint: String 788 is part of the domain name)

## Provided files
* memory_dump.mem

---

## Solution
> **Note:** The output of the commands has been trimmed to keep it short.

I figured this would be similar to the [previous Volatility challenge](../Forensics%20I), and I tried running some plugins immediately. However, Volatility kept screaming at me that it was not working (same, honestly).

Hence, we first have to figure out which profile works for the file by using the `imageinfo` plugin:
```
root@attackdefense:~# vol.py -f memory_dump.mem imageinfo
Volatility Foundation Volatility Framework 2.6.1
INFO    : volatility.debug    : Determining profile based on KDBG search...
          Suggested Profile(s) : Win10x64_10240_17770, Win10x64
✂️--- SNIP ---✂️
```

From this point onwards, we can use the `Win10x64_10240_17770` or `Win10x64` profiles.

### 1. Find filename of compromised server
From the challenge statement, we can see that the exploited process launched multiple Firefox processes. Therefore, to find the exploited processes, we should find the parent process of the Firefox processes.

We can view parent-child process relations easily using the `pstree` plugin:
```
root@attackdefense:~# vol.py -f memory_dump.mem --profile=Win10x64 pstree
Volatility Foundation Volatility Framework 2.6.1
Name                                                  Pid   PPid   Thds   Hnds Time
-------------------------------------------------- ------ ------ ------ ------ ----
✂️--- SNIP ---✂️
.... 0xffffe00053b83080:badblue.exe                  2816   3224      6      0 2019-06-28 05:40:40 UTC+0000
..... 0xffffe00050f4c840:cmd.exe                     1664   2816      0 ------ 2019-06-28 06:09:11 UTC+0000
..... 0xffffe000541a2080:firefox.exe                 4316   2816     50      0 2019-06-28 05:40:41 UTC+0000
...... 0xffffe00053aa1840:firefox.exe                3284   4316     17      0 2019-06-28 06:13:44 UTC+0000
...... 0xffffe0005165f780:firefox.exe                 988   4316     17      0 2019-06-28 05:40:45 UTC+0000
...... 0xffffe00053938080:firefox.exe                2588   4316     18      0 2019-06-28 05:40:44 UTC+0000
...... 0xffffe00053934080:firefox.exe                 548   4316      0 ------ 2019-06-28 05:40:45 UTC+0000
...... 0xffffe00050c5d080:firefox.exe                 676   4316      7      0 2019-06-28 05:40:42 UTC+0000
...... 0xffffe00053d4d840:firefox.exe                 100   4316     16      0 2019-06-28 05:40:49 UTC+0000
...... 0xffffe0005334e840:firefox.exe                4888   4316     19      0 2019-06-28 06:15:36 UTC+0000
✂️--- SNIP ---✂️
```

We can see that the process launching all of the Firefox processes is `badblue.exe`.

#### Flag 1:
```
badblue.exe
```

### 2. Find port number used by exploited process
To view the network connections, we can use the `netscan` plugin:
```
root@attackdefense:~# vol.py -f memory_dump.mem --profile=Win10x64 netscan           
Volatility Foundation Volatility Framework 2.6.1
Offset(P)          Proto    Local Address                  Foreign Address      State            Pid      Owner          Created
✂️--- SNIP ---✂️
0xe00051a391c0     TCPv4    192.168.43.45:85               192.168.43.92:39873  CLOSE_WAIT       2816     badblue.exe    2019-06-28 06:08:38 UTC+0000
✂️--- SNIP ---✂️
0xe00053656010     TCPv4    0.0.0.0:85                     0.0.0.0:0            LISTENING        2816     badblue.exe    2019-06-28 05:40:40 UTC+0000
✂️--- SNIP ---✂️
0xe000537cf7c0     TCPv4    192.168.43.45:5000             192.168.43.92:34535  ESTABLISHED      2816     badblue.exe    2019-06-28 06:08:41 UTC+0000
```

From the list, we can find the exploited process (`badblue.exe`) and see the port number it uses.

#### Flag 2:
```
85
```

### 3. Find core process hijacked by attackers
We can use the `malfind` plugin to find this.

However, during the CTF, it somehow didn't cross my mind to try that plugin, so I looked up core Windows processes and found [this page](https://www.andreafortuna.org/2017/06/15/standard-windows-processes-a-brief-reference/) that pointed out `winlogon.exe` is often used by malware to run things on the shell. Lo and behold, it was, indeed, the answer. 🤷‍♂️

#### Flag 3:
```
winlogon.exe
```

### 4. Find domain name of accessed portal
To find this, I dumped the strings of the memory dump and searched for a web link containing `788`:
```
root@attackdefense:~# strings memory_dump.mem >> str.txt
root@attackdefense:~# cat str.txt | cut --delimiter=' ' -f2 | grep -oE 'https?://.*788.*\..*'
http://www.random-788-registered.com/
✂️--- SNIP ---✂️
```

#### Flag 4:
```
random-788-registered.com
```

> **Note:** I might have misremembered some of the flags (Flag 2?) – the rest of the write-up still applies, but I did a handful of tries of similar answers and forgot to note down the exact flags that were correct. If I did mess up, please let me know (open an issue or something). Thanks! :)