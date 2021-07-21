# Bypass

## Challenge Description
Finally, we have found an interesting web server used by the GDC, but I don’t have credentials so I can’t bypass the login page. I heard that you are the master of injections.

Target URL: http://X.X.X.X/D6IS1E8 [IP redacted]

---

## Solution
Visiting the provided URL takes us to this login page:

<!--screenshot-->

We don't have any credentials to log in with, and entering in some random text gives us this message:

<!--screenshot-->

"No rows" eh? That tells us it's reading from a table, so let's try to SQL inject this login.

The basic concept of an SQL injection is that in a legitimate login, the form would send a query that might look something like this:
```sql
SELECT Username, SomeOtherColumn FROM TableName WHERE Username='$username' AND Password='$password' ORDER BY Username;
```

where the username and password are substituted into the query. We can exploit this by entering part of a query into the username or password field to make the query do what we want it to.

For example, if we inject `e' OR 'a'='a';--` into the username field, the final substituted query will look like this:
```sql
SELECT Username, SomeOtherColumn FROM TableName WHERE Username='someUsername' AND Password='e' OR 'a'='a';--' ORDER BY Username;
```
The part after the semicolon is commented out, and the query matches all of the rows since `1=1` will always be true.

Our aim here is to bypass the login, so we have to inject something that will return a match with any credentials. Here's the one I tried:
```sql
e' OR 'a'='a
```

Here, not closing the quote in `'a'=a` means we don't need to comment out the end.

I tried injecting this into the username field, but it did not work. However, injecting this into the password field gives us a different result:

<!--screenshot-->

Too many matches. So, we have to limit the number of matches it returns, by using `LIMIT` in our query. However, this means that we now have to comment out the end of the query.

SQL comments either start with `--` or `#` (for MySQL). First, I tried adding `--` at the end:
```sql
e' OR 'a'='a' LIMIT 1 --
```

This did not return anything. Perhaps they filtered out `--`? Next, I tried `#`:
```sql
e' OR 'a'='a' LIMIT 1 #
```

<!--screenshot-->

This gives us a result. However, it's not the correct row.

Conveniently, `LIMIT` accepts a range. For example, `LIMIT 2,1` would return `1` match, starting from result `2`, which would return the 3rd result.

Next, I tried:
```sql
e' OR 'a'='a' LIMIT 2,1 #
```

This returned *another* John.
```
[[3,"john","cDR21afzss8"]]
```

Trying more rows, we finally get our flag:

<!--screenshot-->

### Flag:
```
CDDC21{-Inject_Me-}
```
