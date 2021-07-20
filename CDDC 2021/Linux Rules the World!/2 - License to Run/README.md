# Linux 2 – License to Run

## Challenge Description
Glad to hear that you are in. `bot2` has been sent to release some malicious files in a few days. Can you find and check it? 		

## Note
The Linux challenges are consecutive, starting from [part 1](../1%20-%20Lock%20and%20Key).

---
## Solution
First, we must log in to `bot2`, using the previous flag as the password, and then change directory to `bot2`'s home directory:
```
bot1@cybot01:~$ su bot2
Password:
bot2@cybot01:/home/bot1$ cd ~
```

Let's see what files there are here:
```
bot2@cybot01:~$ ls
```

At first glance, there aren't any files. However, let's see if there's anything hidden:
```
bot2@cybot01:~$ ls -a
 .  '.#flag$!!1'   ..   .bash_history   .bash_logout   .bashrc   .profile     
```

Aha! A hidden file.

At this point, I could have done `ls -la` to list all the files *and* their permissions – it slipped my mind to check those during the CTF, and I jumped straight to executing the flag file. (I'm guessing it would have only execute permissions and no read/write perms for `bot2`.)

```
bot2@cybot01:~$ ./'.#flag$!!1' 
CDDC21{TH4nKsF0R_p3RM}
```

### Flag:
```
CDDC21{TH4nKsF0R_p3RM}
```

[< Part 1](../1%20-%20Lock%20and%20Key) | [Part 3 >](../3%20-%20Historian)
