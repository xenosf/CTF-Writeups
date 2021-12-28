# Exploring The Universe! (Part 2)

**Category**: Forensics

382 points | 5 solves

----

## Challenge Description
Oh noes! It seems like Agent Myat has some rather fun teammates, and theres something they're trying to hide!

Look for the clues and try to uncover what he has been doing lately.

Remember, nothing is really deleted on the Internet.

----

## Solution
Reading the blog contents, we can see that there seems to have been a deleted blog post:

<!--image-->

*(and a hint about Git: "#gitgood")*

Using the IPFS Desktop app and the `Qm...` hash from [Part 1](../../Web/Exploring%20The%20Universe%21%20%28Part%201%29), we can access the site's files and see a `git` folder:

<!--image-->

After downloading the files, we can explore the git repository. First, we have to rename the `git` folder to `.git`, then re-initialise the repository:

```sh
$ mv git .git
$ git init
Reinitialized existing Git repository in .../QmZ15yK5GE7gKzKSNC2yj9nLWwNH7sbgyyFcE8pSvzSLMQ/.git/
```

Then, we can take a look at the commit history using `git log`:

```
commit 67a74c89c31e663869e9a5d122989842e58b6949 (HEAD -> master)
Author: willi123yao <willi123yao@gmail.com>
Date:   Sun Dec 26 00:23:41 2021 +0800

    Create another blog post...

commit 2aa8f3958c8a28239c3227c50ac6ae0cf97cc3f4
Author: willi123yao <willi123yao@gmail.com>
Date:   Sun Dec 26 00:23:00 2021 +0800

    Redact a blog post

commit cb35e8620df28a090e7673055559e18137d8caba
Author: willi123yao <willi123yao@gmail.com>
Date:   Sun Dec 26 00:21:03 2021 +0800

    Include some more blog posts

commit 5e8b6225fe0a7b2fe2b4d86efc61bcc7f1c67b38
Author: willi123yao <willi123yao@gmail.com>
Date:   Sun Dec 26 00:20:23 2021 +0800

    Add some blog posts

commit e9d76e869dd00e2605931fe1bccc8d1aba48312b
Author: willi123yao <willi123yao@gmail.com>
Date:   Sun Dec 26 00:20:03 2021 +0800

    Make a blog post page

commit d716ab0800c626090159a8d11949f4715ed8cca6
Author: willi123yao <willi123yao@gmail.com>
Date:   Sun Dec 26 00:19:41 2021 +0800

    Create a wonderful webpage
```

From this, we can see that a blog post had been redacted in commit `2aa8f39...`. To see the redacted posts, we need to see the contents of the commit prior to that, `cb35e86...`.

There are multiple ways to do this, but I opted to make a new branch `old` using commit `cb35e86` as the start point, then use `git diff` to expose the differences:

<!--image-->



### Flag:
```
IRS{G3TT1NG-G00D-1S-the-WAY-T0-5UCC35S!}
```

[< Part 1](../../Web/Exploring%20The%20Universe%21%20%28Part%201%29)
