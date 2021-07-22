# Easy Access

## Challenge Description
I have another mission for you. Check this IP address and see if they left anything useful you can put your hands on.

Target IP Address: *X.X.X.X* [redacted]

---

## Solution
We can use `nmap` to scan for open ports:
```
$ nmap X.X.X.X
Starting Nmap 7.91 ( https://nmap.org ) at 2021-06-23 21:22 +08
Nmap scan report for ec2-X-X-X-X.ap-southeast-1.compute.amazonaws.com (X.X.X.X)
Host is up (0.023s latency).
Not shown: 967 filtered ports, 30 closed ports
PORT    STATE SERVICE
21/tcp  open  ftp
22/tcp  open  ssh
445/tcp open  microsoft-ds

Nmap done: 1 IP address (1 host up) scanned in 73.47 seconds
```

Port 21 is open for FTP! Let's use an FTP client to connect without any authentication:

<img width="1265" alt="Screenshot 2021-06-23 at 21 33 32" src="https://user-images.githubusercontent.com/40383042/126361378-c76b6ee3-f645-4e21-893a-5e1ffffb5233.png">

Here, we find `note.txt`:
```
John, I set a temporary password for you so you can access to your shared folder.
Plz don't put there any sensitive information. TheKeepers might find it somehow!

john:TempTemp123!
```

Aha! Much security. We now have a username and password – but what do we use them for?

From the earlier `nmap` output, we can see that port `445` is also open, for `microsoft-ds`, which is used for [SMB (Server Message Block)](https://en.wikipedia.org/wiki/Server_Message_Block).

Knowing this, we can connect to port `445` using the credentials we found earlier, and retrieve the flag:

<img width="992" alt="Screenshot 2021-06-23 at 21 31 40" src="https://user-images.githubusercontent.com/40383042/126361391-9dd88cd3-8163-4b41-a37f-b3598b21c5ff.png">


### Flag:
```
CDDC21{H0w_d1d_y0u_GET_he4e?}
```
