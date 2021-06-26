# DevSecOps Scenario III

## Challenge Description
DevOps practices are to combine software development (Dev) and IT operations (Ops) in order to improve the delivery process. DevOps pipelines are chained tasks and components that run in a sequence to cover different phases of software compilation, packaging, automated testing, and test deployment.  

In this lab, we have a DevSecOps pipeline for a Django web application.

**Objective:** Fix the Issues in the stages of the pipeline and Find the flags!

## Instructions
* The GitLab server is reachable with the name 'gitlab'
* [Gitlab credentials provided]
* [ArcherySec credentials provided]

---

## Solution

> **Note:** I forgot to take screenshots for this or copy the source code, so this write-up isn't as detailed as I'd like it to be. Hopefully what I have here is enough for you to understand how I got the flags.

This challenge is similar to [DevSecOps Scenario I](../DevSecOps%20Scenario%20I).

Running the lab opens up a page with links to VS Code, Jenkins, ArcherySec, and GitLab instances which are all running locally on the lab server, as well as a link to the web app.

From the given resources, we can see that the code for this project is first pushed to the repository on GitLab, then automatically built and tested using Jenkins. We can see the full build pipeline on the build pipeline page of Jenkins, showing us the many stages it will go through before being deployed.

We can access the Git repository using `http://gitlab/root/django-blog.git`.

### Build Test 1: DevSkim
To find out what is wrong, we start a build on Jenkins using the existing code.

Doing so outputs to the console, which we can view using the Jenkins web interface:
```
✂️--- SNIP ---✂️
[devskim - scan] $ /bin/sh -xe /tmp/jenkins130254802635285484.sh
+ /devskim.sh
file:./.aws/credentials.txt
	region:2,25,2,70 - ds173237 [important] - do not store tokens or keys in source code.

issues found: 1 in 1 files
✂️--- SNIP ---✂️
build step 'execute shell' marked build as failure
finished: failure
```
> **Note:** The above console output has been trimmed to keep it short. Full output for the build tests mentioned in this write-up can be found in this folder.

The test highlighted an issue in `./.aws/credentials.txt`.

To fix this, we must first clone the code repository, by opening VS Code's integrated terminal (ctrl-\`) and using this command:
```
git clone http://gitlab/root/django-blog.git
```

Now that we have the code on our machine, we can find the aforementioned file and delete it. (In real life you'd move it to somewhere more appropriate, such as a `.env` file.)
```
git rm ./.aws/credentials.txt
git commit -m "deleted aws credentials"
git push origin master
```

When the code is pushed to the GitLab repository, a build is automatically started. We can view the results of the latest build on Jenkins:
```
✂️--- SNIP ---✂️
[Devskim - Scan] $ /bin/sh -xe /tmp/jenkins12862511298467905034.sh
+ /devskim.sh
Issues found: 0 in 0 files
✂️--- SNIP ---✂️
FLAG 1: 8aa6cc853503696b5740a462b57aebb6
Triggering a new build of Truffle Hog - Scan
Finished: SUCCESS
```

#### Flag 1:
```
8aa6cc853503696b5740a462b57aebb6
```

### Build Test 2: TruffleHog 
Now that we have fixed the first issue, we can take a look at the second stage's output:
```
[Truffle Hog - Scan] $ /bin/sh -xe /tmp/jenkins851098456940431230.sh
+ /trufflehog.sh
✂️--- SNIP ---✂️
  "path": ".aws/credentials.txt",
  "printDiff": "@@ -0,0 +1,3 @@\n+aws_access_key_id = 123456789012 \n+aws_secret_access_key = '\u001b[93mfebcc175b9c0f1b6a831c399eadd2912697726617de\u001b[0m' \n+aws_session_token = '\u001b[93mAQoEXAMPLEH4aoAH0gNCAPyJxz4BlCFFxWNE1OPTgk5TthT+FvwqnKwRcOIfrRh3c/LTo6UDdyJwOOvEVPvLXCrrrUtdnniCEXAMPLE/IvU1dYUg2RVAJBanLiHb4IgRmpRV3zrkuWJOgQs8IZZaIv2BXIa2R4Olgk\u001b[0m'\n",
  "reason": "High Entropy",
✂️--- SNIP ---✂️
  "path": "templates/base.html",
  "printDiff": "@@ -1,37 +0,0 @@\n-{% load static %}\n-<!DOCTYPE html>\n-<html lang=\"en\">\n-\n-<head>\n-    <meta charset=\"UTF-8\">\n-    <meta name=\"viewport\" content=\"width=device-width, initial-scale=1.0\">\n-    <meta http-equiv=\"X-UA-Compatible\" content=\"ie=edge\">\n-    <title>Poll Me | Polls List</title>\n-    <link rel=\"stylesheet\" href=\"https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/css/bootstrap.min.css\" integrity=\"sha384-\u001b[93mGn5384xqQ1aoWXA+058RXPxPg6fy4IWvTNh0E263XmFcJlSAwiGgFAW/dAiS6JXm\u001b[0m\"\n-        crossorigin=\"anonymous\">\n-    <link rel=\"stylesheet\" href=\"https://use.fontawesome.com/releases/v5.6.3/css/all.css\" integrity=\"sha384-\u001b[93mUHRtZLI+pbxtHCWp1t77Bi1L4ZtiqrqD80Kn4Z8NTSRyMA2Fd33n5dQ8lWUE00s/\u001b[0m\"\n-        crossorigin=\"anonymous\">\n-\n-    {% block custom_css %}{% endblock custom_css %}\n-\n-</head>\n-\n-<body>\n-    {% include 'includes/navbar.html' %}\n-\n-    {% block content %}\n-\n-    {% endblock content %}\n-\n-\n-    <!-- All javascript files goes under here    -->\n-    <script src=\"https://code.jquery.com/jquery-3.2.1.slim.min.js\" integrity=\"sha384-\u001b[93mKJ3o2DKtIkvYIK3UENzmM7KCkRr/rE9/Qpg6aAZGJwFDMVNA/GpGFF93hXpG5KkN\u001b[0m\"\n-        crossorigin=\"anonymous\"></script>\n-    <script src=\"https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.12.9/umd/popper.min.js\" integrity=\"sha384-\u001b[93mApNbgh9B+Y1QKtv3Rn7W3mgPxhU9K/ScQsAP7hUibX39j7fakFPskvXusvfa0b4Q\u001b[0m\"\n-        crossorigin=\"anonymous\"></script>\n-    <script src=\"https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/js/bootstrap.min.js\" integrity=\"sha384-\u001b[93mJZR6Spejh4U02d8jOt6vLEHfe/JQGiRRSQQxSfFWpi1MquVdAyjUar5+76PVCmYl\u001b[0m\"\n-        crossorigin=\"anonymous\"></script>\n-\n-</body>\n-\n-</html>\n\\ No newline at end of file\n",
  "reason": "High Entropy",
✂️--- SNIP ---✂️
  "path": ".aws/credentials.txt",
  "printDiff": "@@ -0,0 +1,3 @@\n+aws_access_key_id = 123456789012 \n+aws_secret_access_key = '\u001b[93mfebcc175b9c0f1b6a831c399eadd2912697726617de\u001b[0m' \n+aws_session_token = '\u001b[93mAQoEXAMPLEH4aoAH0gNCAPyJxz4BlCFFxWNE1OPTgk5TthT+FvwqnKwRcOIfrRh3c/LTo6UDdyJwOOvEVPvLXCrrrUtdnniCEXAMPLE/IvU1dYUg2RVAJBanLiHb4IgRmpRV3zrkuWJOgQs8IZZaIv2BXIa2R4Olgk\u001b[0m'\n",
  "reason": "High Entropy",
✂️--- SNIP ---✂️
Build step 'Execute shell' marked build as failure
Finished: FAILURE
```

We can see that it has highlighted 2 files:
* `./.aws/credentials.txt`
* `./templates/base.html`

We have already removed `./.aws/credentials.txt` in the earlier step. Next, we have to deal with `./templates/base.html`.

From the console output, we can see that the following tags have been flagged, due to the `integrity` attribute on the `<link` and `<script>` tags. Were this a real life scenario, this would be a false positive of the test as it is not sensitive information – in fact, this attribute improves security by ensuring the imported 3rd-party scripts have not been changed. ([More reading](https://developer.mozilla.org/en-US/docs/Web/Security/Subresource_Integrity) if you're interested)

However, in this case, we can't change the build tests themselves to whitelist these (and it's a fake scenario), so I just deleted the `integrity` attributes and pushed the changes.
```
git add ./templates/base.html
git commit -m "woo! security!"
git push origin master
```

> **Note:** The following steps are nearly identical to what I did for the earlier DevSecOps flag, so I won't elaborate much here. My [DevSecOps I write-up](../DevSecOps%20Scenario%20I#build-test-2-trufflehog) goes into more detail.

We're not done yet! TruffleHog searches the Git commit history, which means we have to combine the commits so that these strings do not show up in the diffs.

We can do this by squashing the commits:
```
git rebase -i --root
```

After squashing the commits, we can unprotect the `master` branch on GitLab and force push the changes.
```
git push -f origin master
```

Going back to Jenkins, we see that the 2nd stage has succeeded.
```
✂️--- SNIP ---✂️
[Truffle Hog - Scan] $ /bin/sh -xe /tmp/jenkins2826092293007452953.sh
+ /trufflehog.sh
Cloning into 'django-blog'...
FLAG 2: cb4f2a3413adc795a5088dd7e7e01a2f
Triggering a new build of Bandit
Finished: SUCCESS
```

#### Flag 2:
```
cb4f2a3413adc795a5088dd7e7e01a2f
```

### Build Test 3: Bandit
Moving onto the next stage, we can see that it has failed.
```
[Bandit] $ /bin/sh -xe /tmp/jenkins15514664569618937391.sh
✂️--- SNIP ---✂️
Test results:
>> Issue: [B605:start_process_with_a_shell] Starting a process with a shell, possible injection detected, security issue.
   Severity: High   Confidence: High
   Location: ./seeder.py:102
   More Info: https://bandit.readthedocs.io/en/latest/plugins/b605_start_process_with_a_shell.html
101	    datestamp=""        
102	    os.system(datestamp)
103	    print()

✂️--- SNIP ---✂️
Build step 'Execute shell' marked build as failure
Finished: FAILURE
```

From this, we can see that the file `./seeder.py` has a security issue, due to the `os.system(datestamp)` line.

To fix this, we can simply delete that line (it doesn't appear to do anything useful anyways), and push the changes.

This time, the build succeeds:
```
✂️--- SNIP ---✂️
[Bandit] $ /bin/sh -xe /tmp/jenkins3536438064475983714.sh
✂️--- SNIP ---✂️
FLAG 3: f0ed5839d6addaf6eebc5fc5750ede1c
Triggering a new build of Safety
Finished: SUCCESS
```

#### Flag 3:
```
f0ed5839d6addaf6eebc5fc5750ede1c
```

### Build Test 4: Safety
Let's move onto the next stage, which, predictably, fails (are you bored yet?):
```
[Safety] $ /bin/sh -xe /tmp/jenkins13354682711613193132.sh
+ /safety.sh
✂️--- SNIP ---✂️
| REPORT                                                                       |
| checked 5 packages, using local DB                                           |
+============================+===========+==========================+==========+
| package                    | installed | affected                 | ID       |
+============================+===========+==========================+==========+
| pillow                     | 6.0.0     | <6.2.2                   | 37782    |
| pillow                     | 6.0.0     | <6.2.2                   | 37781    |
| pillow                     | 6.0.0     | <6.2.2                   | 37780    |
| pillow                     | 6.0.0     | <6.2.2                   | 37779    |
| pillow                     | 6.0.0     | <6.2.3                   | 38449    |
| pillow                     | 6.0.0     | <6.2.3                   | 38450    |
| pillow                     | 6.0.0     | <7.0.0                   | 38451    |
| pillow                     | 6.0.0     | <=7.0.0                  | 38452    |
+==============================================================================+
Build step 'Execute shell' marked build as failure
Finished: FAILURE
```

This tells us that the (old) version of the `pillow` package that this project uses has vulnerabilities. To fix it, we must use a newer version.

This can be done by editing `requirements.txt`, and changing the version of `pillow` used.

Change
```
pillow=6.0.0
```
to:
```
pillow=7.1.0
```

In Jenkins, we can see now that the build has succeeded and this test succeeds:
```
[Safety] $ /bin/sh -xe /tmp/jenkins2793373912626157167.sh
+ /safety.sh
| REPORT                                                                       |
| checked 5 packages, using local DB                                           |
+==============================================================================+
| No known security vulnerabilities found.                                     |
+==============================================================================+
FLAG 4: 46d5b8115e47963392f66c5b6941c2e0
Triggering a new build of Django Application Installation
Finished: SUCCESS
```

#### Flag 4:
```
46d5b8115e47963392f66c5b6941c2e
```

The next few stages succeed, so let's skip to the next one that fails.

### Build Test 8: Inspec
Oh no the 2nd last one failed :( so close
```
[Inspec - Compliance] $ /bin/sh -xe /tmp/jenkins16818181110745954727.sh
+ chmod +x /inspec.sh
+ /inspec.sh
✂️--- SNIP ---✂️

[38;5;9m  ��  tomcat.directories: Check for existence and correct permissions of application directory (1 failed)[0m
[38;5;41m     ���  Directory /home/tomcat/app/ is expected to be directory[0m
[38;5;41m     ���  Directory /home/tomcat/app/ owner is expected to eq "tomcat"[0m
[38;5;9m     ��  Directory /home/tomcat/app/ mode is expected to cmp == "0750"
     
     expected: 0750
          got: 0755
     
     (compared using `cmp` matcher)
[0m

✂️--- SNIP ---✂️
Build step 'Execute shell' marked build as failure
Finished: FAILURE
```

This test has flagged 3 issues:
1. `/home/tomcat/app/` should be a directory
2. `/home/tomcat/app/` should be owned by `tomcat`
3. `/home/tomcat/app/` should have permissions mode `0750` instead of `0755`

Poking around in the project, we can find `django.yml`, which looks like it's the cause of the aforementioned issues.

> **Note**: Unfortunately, I lost the original file so I can only show the edited version.

**Fixed file:**
```yaml
---
- hosts: test-server                                       
      become: yes
      tasks:

       - name: Checking for old files        # Running the tests to check for Old django installation
         stat:
           path: /home/tomcat/app/
         register: file

       - name: Removing the old files        # Deleting the old django files
         command: "{{ item }} "
         with_items:
         - killall python3 2>/dev/null
         - rm -rf /home/tomcat/app/
         args:
            warn: no
         when: file.stat.isdir is defined

       - name: Creating Directory             # Placing new files in the machine
         file:
          path: /home/tomcat/app
          state: directory

       - name: Extracting Django
         unarchive:
          src: /tmp/django.tar.gz
          dest: /home/tomcat/app/

       - name: Configuring Django            # Giving permissions to the tomcat installation and scripts
         command: "{{ item }} "
         with_items:
         - chmod 750 /home/tomcat/app/
         - chown -RH tomcat:tomcat /home/tomcat/app/
         - chmod +x /home/tomcat/app/startup.sh
         args:
            warn: no
   
       - name: Starting Django               # Starting the django server
         command: nohup /home/tomcat/app/startup.sh   
         args:
          warn: no
```

Here's the changes I made to fix the issues:
1. Changed `file` to `directory`
2. Changed (or added? I forgot lol) the `chown` command to make `tomcat:tomcat` the owner
3. Added the `chmod 750 /home/tomcat/app/` line to change permissions of the folder

After committing and pushing the changes, we can see that it succeeds and finally gets built!

```
[Inspec - Compliance] $ /bin/sh -xe /tmp/jenkins10997141552396738775.sh
+ chmod +x /inspec.sh]
+ /inspec.sh
✂️--- SNIP ---✂️

[38;5;41m  ���  tomcat.directories: Check for existence and correct permissions of application directory[0m
[38;5;41m     ���  Directory /home/tomcat/app/ is expected to be directory[0m
[38;5;41m     ���  Directory /home/tomcat/app/ owner is expected to eq "tomcat"[0m
[38;5;41m     ���  Directory /home/tomcat/app/ mode is expected to cmp == "0750"[0m


✂️--- SNIP ---✂️
FLAG 5: 3d5ef9972e4954dd883106240858bf6b
Finished: SUCCESS
```

#### Flag 5:
```
3d5ef9972e4954dd883106240858bf6b
```

Sadly, at this point, I was so sick of this that I forgot to look at the final web app before closing the lab. Oh well. Flag is flag.
