# Network Recon III

## Challenge Description

A web application uses Redis server as the database server. The attacker has sneaked into the network on which the Redis server is present.

Please answer the following questions:

1. What message is being published on the active channel of the Redis server?
2. Find the key which contains the hash field 'authorization_key'.
3. Find the value of hash field 'admin_password' stored at the 'settings' key.

## Instructions

* Once you start the lab, you will have access to a root terminal of a Kali instance
* Your Kali has an interface with IP address 192.X.Y.2. Run "ip addr" to know the values of X and Y.
* The Target machine should be located at the IP address 192.X.Y.3.

---

## Solution

Firstly, we have to find the IP address of the target machine by following the instructions. In  this case, it was `192.97.36.3`.

Then, we have to connect to the Redis server. Being completely unfamiliar with Redis, I turned to my good old friend DuckDuckGo (substitute with any other search engine of your choice) to find some documentation.

I found that the default port was 6379, and that we can use `redis-cli` to interact with the server:

```text
root@attackdefense:~# redis-cli -h 192.97.36.3 -p 6379
192.97.36.3:6379> ping
PONG
```

The ping isn't necessary &ndash; I used it to see whether I'd connected properly.

### 1. Find message being published on the active channel

Looking through the documentation, I found [this page in the documentation](https://redis.io/topics/pubsub) that explains how Redis implements the "Publish/Subscribe messaging paradigm", as well as links to relevant command documentation.

To see the active channels, we can use the `PUBSUB` command:

```text
192.97.36.3:6379> PUBSUB CHANNELS
1) "notifications"
```

We can then subscribe to the "notifications" channel to read the messages being published.

```text
192.97.36.3:6379> SUBSCRIBE notifications
Reading messages... (press Ctrl-C to quit)
1) "subscribe"
2) "notifications"
3) (integer) 1
1) "message"
2) "notifications"
3) "Password for user 'admin' was updated."
^C
```

#### Flag 1

```text
Password for user 'admin' was updated.
```

### 2. Find key containing hash field `authorization_key`

After doing `Ctrl-C` to get out of the message display earlier, we have to reconnect to the server.

Then, we can list all keys:

```text
192.97.36.3:6379> keys *
 1) "users"
 2) "greetings"
 3) "is-offline"
 4) "home_page"
 5) "config"
 6) "statistics"
 7) "active_api_keys"
 8) "blacklisted-ip"
 9) "settings"
10) "data
```

Some keys can be hash keys, which contain multiple fields. For example, in this case, `settings` is a hash key:

```text
192.97.36.3:6379> type settings
hash
```

We can list the contents of this hash key:

```text
192.97.36.3:6379> hgetall settings
 1) "username"
 2) "admin"
 3) "contact-no"
 4) "4123454165"
 5) "role"
 6) "sys_admin"
 7) "admin_password"
 8) "superSecretPass@123"
 9) "last_login"
10) "12/06/2019"
11) "email"
12) "admin@recon-badge.local"
```

This particular key does not contain the field we are looking for. Let's look for another hash key:

```text
192.97.36.3:6379> type users
list
192.97.36.3:6379> type greetings
string
192.97.36.3:6379> type is-offline
string
192.97.36.3:6379> type home_page
string
192.97.36.3:6379> type config
hash
```

Now that we know that the key `config` is of type `hash`, let's dump the contents of `config`:

```text
192.97.36.3:6379> hgetall config
 1) "authorization_key"
 2) "d9bd6d9150d3f7004830819da569aa9a"
 3) "endpoint"
 4) "api.recon-badge.local/private"
 5) "parameter"
 6) "data"
 7) "method"
 8) "POST"
 9) "parameter_type"
10) "json"
```

We can see that it lists the field name, then the value, like so:

```text
1) "FIELD NAME 1"
2) "VALUE 1"
3) "FIELD NAME 2"
4) "VALUE 2"
...and so on.
```

Thus, we can get the value of `authorization_key` from the output above.

#### Flag 2

```text
d9bd6d9150d3f7004830819da569aa9a
```

### 3. Find value of hash field `admin_password` stored at `settings` key

Earlier, we got the contents of the `settings` key:

```text
192.97.36.3:6379> hgetall settings
 1) "username"
 2) "admin"
 3) "contact-no"
 4) "4123454165"
 5) "role"
 6) "sys_admin"
 7) "admin_password"
 8) "superSecretPass@123"
 9) "last_login"
10) "12/06/2019"
11) "email"
12) "admin@recon-badge.local"
```

We can see that the value of the `admin_password` field is `superSecretPass@123` (Tsk, plaintext password storage again...)

#### Flag 3

```text
superSecretPass@123
```
