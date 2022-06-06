# Forensics I

## Challenge Description

A memory dump is provided in the home directory of the root user. You have to use Volatility to  analyze the memory dump and answer the following questions:

1. What was the PID of process rsyslog?
2. How many threads were spawned by the process rsyslog?
3. The insmod command which is used to insert kernel module, is fired from which directory? Provide the absolute path.
4. What is the name of the parent process of process 'upstart'?
5. What is the MAC address of the machine which had IP address 192.168.8.167 assigned to it?
6. The memory dump was taken using LiME tool using a TCP socket. What is the port number used for this socket?
7. What was the mount point for /dev/sda2?
8. There is a kernel module that is used by a popular virtualization solution to provide additional services to guest OS. This module was running at the time of taking the memory capture. What is the name of that kernel module?

## Provided files

* memory_dump.img

---

## Solution

> **Note:** The output of the commands has been trimmed to keep it short.

Firstly, I did `vol.py -h` to see a list of available plugins (also because I may or may not have forgotten how to use Volatility). I referred back to the help message throughout the course of trying to find the flags.

### 1. Find PID of process `rsyslog`

We can use `pslist` to (I saved the output into a file `pslist.txt` to refer to later

```text
root@attackdefense:~# vol.py -f memory_dump.img linux_pslist >> pslist.txt
```

Use `grep` to search for the `rsyslog` process:

```text
root@attackdefense:~# cat pslist.txt | grep rsyslog
0xffff9636cf7796c0 rsyslogd             644             1               104             108    0x0000000012402000 0
```

The PID of the process is the 3rd column.

#### Flag 1

```text
644
```

### 2. Find number of threads spawned by the process `rsyslog`

Use the `linux_threads` plugin to list the threads:

```text
root@attackdefense:~# vol.py -f memory_dump.img linux_threads -p 644      
Volatility Foundation Volatility Framework 2.6.1

Process Name: rsyslogd
Process ID: 644
Thread PID    Thread Name     
------------- ----------------
644           rsyslogd        
648           in:imuxsock     
649           in:imklog       
650           rs:main Q:Reg  
```

We can see that there are 4 threads.

#### Flag 2

```text
4
```

### 3. Find current working directory of `insmod`

There's a plugin for that, too!

```text
root@attackdefense:~# vol.py -f memory_dump.img linux_getcwd -p 14862
Volatility Foundation Volatility Framework 2.6.1
Name              Pid      CWD
----------------- -------- ---
insmod               14862 /root/LiME/src
```

#### Flag 3

```text
/root/LiME/src
```

### 4. Find name of parent process of `upstart`

ppid 1073
From our earlier saved `pslist.txt`, we can look for the `upstart` process:

```text
root@attackdefense:~# cat pslist.txt | grep upstart
0xffff9636d4090000 upstart              1296            1073            1000            1000   0x00000000139a6000 0
✂️--- SNIP ---✂️
```

The `PPID` - parent process ID - is the 4th column: `1073`.

Then, we can look for the name of the parent process in `plist.txt`:

```text
✂️--- SNIP ---✂️
0xffff9636de5096c0 lightdm              802             1               0               0      0x00000000109d2000 0
✂️--- SNIP ---✂️
```

#### Flag 4

```text
lightdm
```

### 5. Find MAC address of machine at `192.168.8.167`

We can get the ARP (Address Resolution Protocol) table. The ARP table stores the discovered MAC and IP addresses of machines connected to a network. ([more info](https://www.auvik.com/franklyit/blog/what-is-an-arp-table/)) 
```
root@attackdefense:~# vol.py -f memory_dump.img linux_arp            
Volatility Foundation Volatility Framework 2.6.1
✂️--- SNIP ---✂️
[192.168.8.167                             ] at d0:04:01:01:fd:ba    on enp0s3
✂️--- SNIP ---✂️
```

#### Flag 5

```text
d0:04:01:01:fd:ba
```

### 6. Find port number used for LiME TCP socket

From the [earlier flag](#3-find-current-working-directory-of-insmod), we can see that the process ID for LiME was `14862`. We can investigate that process in more detail using the `linux_psaux` plugin, and see the port number in the arguments of the command used:

```text
root@attackdefense:~# vol.py -f memory_dump.img linux_psaux -p 14862     
Volatility Foundation Volatility Framework 2.6.1
Pid    Uid    Gid    Arguments                                                       
14862  0      0      insmod lime-4.15.0-45-generic.ko path=tcp:5000 format=lime
```

#### Flag 6

```text
5000
```

### 7. Find mount point for `/dev/sda2`

We can use the `linux_mount` plugin to see the mounted devices:

```text
root@attackdefense:~# vol.py -f memory_dump.img linux_mount              
Volatility Foundation Volatility Framework 2.6.1
/dev/sda1                 /                                   ext4         rw,relatime                                                       
/dev/sda2                 /boot                               ext4         rw,relatime
✂️--- SNIP ---✂️
```

From this, we can find `/dev/sda2` is mounted on `/boot`.

#### Flag 7

```text
/boot
```

### 8. Find name of virtualization solution guest OS kernel module

We can use the plugin `linux_lsmod` to see the loaded kernel modules:

```text
root@attackdefense:~# vol.py -f memory_dump.img linux_lsmod
Volatility Foundation Volatility Framework 2.6.1
✂️--- SNIP ---✂️
ffffffffc04a5200 vboxguest 303104
✂️--- SNIP ---✂️
```

We can find the module with "guest" in the name (and we know that "vbox" likely stands for VirtualBox).

#### Flag 8

```text
vboxguest
```
