# Linux 1 &ndash; Lock and Key

## Challenge Description

One of TheKeepers has successfully obtained what seems to be one of the GDC private servers. He has sent me the image and another file, but unfortunately, I'm not great with Linux. I think you're the one for this mission.

Target: *X.X.X.X* [redacted]

## Attached files

* cybot01_bot1.key
* Note.txt

### Note.txt

```text
Notice

1. As part of the ZIP file, you will find a text file that can be used to connect to the target machine. 

2. Each one of the flags is the password for the next user. Your main goal is to access the last user account.

3. The flags are located in the user home folders.
```

---

## Solution

`cybot01_bot1.key` is a private key file that can be used to SSH into the target machine. Let's use it:

```sh
$ ssh bot1@X.X.X.X -i cybot01_bot1.key
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
Permissions 0644 for 'cybot01_bot1.key' are too open.
It is required that your private key files are NOT accessible by others.
This private key will be ignored.
Load key "cybot01_bot1.key": bad permissions
Password: 
```

It seems like we have to change the permissions of the file to only allow us to access it:

```sh
chmod 600 cybot01_bot1.key
```

The 600 means that our user can read and write the file, while no one else can read, write, or execute it.

Now that we've settled the permissions issue, let's try again:

```sh
$ ssh bot1@X.X.X.X -i cybot01_bot1.key
Welcome to Ubuntu 20.04.2 LTS (GNU/Linux 5.8.0-1035-aws x86_64)

✂️--- SNIP ---✂️

Last login: Wed Jun 23 05:57:20 2021 from Y.Y.Y.Y
```

[Hacker voice] *We're in.*

Now, we can access the home folder, and retrieve the flag:

```sh
bot1@cybot01:~$ ls
flag.txt
bot1@cybot01:~$ cat flag.txt
CDDC21{b0t_eNtR3nC3}
```

### Flag

```text
CDDC21{b0t_eNtR3nC3}
```

[Part 2 >](../2%20-%20License%20to%20Run)
