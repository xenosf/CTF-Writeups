# DevSecOps Scenario I

## Challenge Description
DevOps practices are to combine software development (Dev) and IT operations (Ops) in order to improve the delivery process. DevOps pipelines are chained tasks and components that run in a sequence to cover different phases of software compilation, packaging, automated testing, and test deployment.  

In this lab, we have a partial DevSecOps pipeline for a Django web application. It consists of the following components (and tasks):

* Automated Code Review: DevSkim
* Sensitive Information Scan phase: Truffle Hog

**Objective:** Fix the Issues in the stages of the pipeline and collect all the flags!

## Instructions
* The GitLab server is reachable with the name 'gitlab'
* [Gitlab credentials provided]

---

## Solution

> **Note:** I forgot to take screenshots for this or copy the source code, so this write-up isn't as detailed as I'd like it to be. Hopefully what I have here is enough for you to understand how I got the flags.

Running the lab opens up a page with links to VS Code, Jenkins, and GitLab instances which are all running locally on the lab server.

From the given resources, we can see that the code for this project is first pushed to the repository on GitLab, then built and tested using Jenkins (with 2 build tests, DevSkim and TruffleHog).

We can access the Git repository using `http://gitlab/root/opensource-job-portal.git`.

### Build Test 1: DevSkim
To find out what is wrong, we start a build on Jenkins using the existing code.

Doing so outputs to the console, which we can view using the Jenkins web interface:
```
✂️--- SNIP ---✂️
[Devskim - Scan] $ /bin/sh -xe /tmp/jenkins1756405203869299285.sh
+ /devskim.sh
file:./psite/forms.py
	region:48,31,48,35 - DS126858 [Critical] - Weak/Broken Hash Algorithm

Issues found: 1 in 1 files
✂️--- SNIP ---✂️
Build step 'Execute shell' marked build as failure
Finished: FAILURE
```

> **Note:** The above console output has been trimmed to keep it short. Full output for the build tests can be found in the same folder as this write-up.

To pass the first build test, we must fix the issue highlighted in `./psite/forms.py`, which is a "Weak/Broken Hash Algorithm".

To fix it, we first have to clone the code repository. We can do this by opening VS Code's integrated terminal (ctrl-\`) and using this command:
```
git clone http://gitlab/root/opensource-job-portal.git
```

Then, we find the problematic file, `forms.py`,  which is a form containing a password field. The original implementation of this was a `CharField` with MD5 hashing, which is a weak hashing algorithm and likely what triggered the test failure.

To fix this, I replaced the password field code with a more secure implentation found online:
```python
password = CharField(widget=PasswordInput())
```

I then saved the new code, committed it, and pushed it.
```
git add ./psite/forms.py
git commit -m "changed password field"
git push origin master
```

We can then open Jenkins again and start a new build, which gives us the following output for the first test:
```
✂️--- SNIP ---✂️
[Devskim - Scan] $ /bin/sh -xe /tmp/jenkins7709832272473403515.sh
+ /devskim.sh
Issues found: 0 in 0 files
✂️--- SNIP ---✂️
FLAG 1: 5bbd3d90fb303699b48e378715526c50
Triggering a new build of Truffle Hog - Scan
Finished: SUCCESS
```

#### Flag 1:
```
5bbd3d90fb303699b48e378715526c50
```

### Build Test 2: TruffleHog

Moving on, we can see that the subsequent test has failed.
```
✂️--- SNIP ---✂️
[Truffle Hog - Scan] $ /bin/sh -xe /tmp/jenkins8737012020977949385.sh
+ /trufflehog.sh
✂️--- SNIP ---✂️
  "path": "client_secret.json",
  "printDiff": "\u001b[93m\"client_secret\":\"b7rA_4OWMBshb_t28AU6W7Tf\"\u001b[0m",
  "reason": "Google Oauth",
✂️--- SNIP ---✂️
  "path": "env.md",
  "printDiff": "\u001b[93mhttp://guest:guest@localhost:15672/api/\"\n\u001b[0m",
  "reason": "Password in URL",
✂️--- SNIP ---✂️
  "path": "templates/mobile/jobs/private",
  "printDiff": "\u001b[93m-----BEGIN OPENSSH PRIVATE KEY-----\u001b[0m",
  "reason": "SSH (OPENSSH) private key",
✂️--- SNIP ---✂️
Build step 'Execute shell' marked build as failure
Finished: FAILURE
```

From the above, we can see that 3 files have been highlighted:
* `./client_secret.json`
* `./env.md`
* `./templates/mobile/jobs/private`

To fix this, I went back to VS Code and deleted those files. (In real life you should probably have those files in a separate place anyway)
```
git rm client_secret.json env.md templates/mobile/jobs/private
git commit -m "deleted stuff"
git push origin master
```

Returning to Jenkins and building it again, I found that it still failed, pointing out the same things. But those files are gone! How can dis be allow?

This is because TruffleHog searches in your *Git commit history*, not just the most recent version - it's still finding these secrets in the diffs.

This means that we have to somehow combine all of the commits into one. Luckily, we can do that by squashing the commits:
```
git rebase -i --root
```

This rebases all of the commits onto the first commit (`--root`), which means it applies the changes in the later commits directly onto the first one, combining them. It does this interactively (`-i`).

Entering the above command opens up an editor with the following text:
```
 pick xxxxxxx initial commit
 pick xxxxxxx changed password field
 pick xxxxxxx deleted stuff
✂️--- SNIP ---✂️
```
where `xxxxxxx` is the first few characters of the commit SHA.

To squash these commits, change `pick` to `squash`.
```
 pick xxxxxxx initial commit
 squash xxxxxxx changed password field
 squash xxxxxxx deleted stuff
✂️--- SNIP ---✂️
```

After doing this, we are ready to push. Since we rewrote the commit history, we have to force push it.
```
git push -f origin master
```

However, doing so gives us an error:
```
remote: GitLab: You are not allowed to force push code to a protected branch on this project.
```

To fix this, we have to go to GitLab and unprotect the `master` branch by going to the Protected Branches section in the Settings > Repository page and removing the protection.

After that, we go back to VSCode and force push it again.

Checking in GitLab, we can see that there is only one commit.

Then, we can open Jenkins and build the project again. This time, it succeeds:
```
✂️--- SNIP ---✂️
[Truffle Hog - Scan] $ /bin/sh -xe /tmp/jenkins5821072848807200072.sh
+ /trufflehog.sh
Cloning into 'opensource-job-portal'...
FLAG 2: de88d953d8650af500f76e017d985b62
Finished: SUCCESS
```

#### Flag 2:
```
de88d953d8650af500f76e017d985b62
```