# Linux 4 – Line Inspection

## Challenge Description
The bots were able to encrypt their secrets but we found that they were owned by `bot3`. Are there some traces of human-readable text? 		 		

## Note
The Linux challenges are consecutive, starting from [part 1](../1%20-%20Lock%20and%20Key).

---
## Solution
First, we must log in to `bot4`, using the previous flag as the password, and then change directory to `bot4`'s home directory:
```
bot3@cybot01:~$ su bot4
Password: 
bot4@cybot01:/home/bot3$ cd ~
```

Let's see what we have to work with:
```
bot4@cybot01:~$ ls -la
total 120
dr-xr-x---  2 root bot4  4096 Jun 18 09:51 .
drwxr-xr-x 10 root root  4096 Jun 18 09:51 ..
lrwxrwxrwx  1 root root     9 Jun 18 09:51 .bash_history -> /dev/null
-r--r-----  1 bot4 bot4   220 Feb 25  2020 .bash_logout
-r--r-----  1 bot4 bot4  3771 Feb 25  2020 .bashrc
-r--r-----  1 bot4 bot4   807 Feb 25  2020 .profile
-r--r-----  1 bot4 root 99998 Jun 18 09:51 random-secrets
```

The flag must be in `random-secrets`!
```
bot4@cybot01:~$ strings random-secrets 
Sahqueigh6Zahkoiqu
✂️--- SNIP ---✂️
```

However, the contents are filled with junk. To find the flag needle within this secrets haystack, we can use `grep` to search the strings:
```
bot4@cybot01:~$ strings random-secrets | grep CDDC
CDDC21{gRe3EpL1nG}  
```

### Flag:
```
CDDC21{gRe3EpL1nG}
```

[< Part 3](../3%20-%20Historian) | [Part 5 >](../5%20-%20Super)
