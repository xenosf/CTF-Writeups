# Forensics II

## Challenge Description
A container is running in the lab and a flag file is supposed to be present in the home directory of the root user. The flag is a string of 32 hexadecimal characters.

**Objective:** Retrieve the flag!

---

## Solution
To view the running containers, we can use `docker ps`:
```
student@localhost:~$ docker ps
CONTAINER ID        IMAGE               COMMAND               CREATED             STATUS              PORTS               NAMES
8932bbf90332        ubuntuderived       "tail -f /dev/null"   12 minutes ago      Up 12 minutes                           mod-ubuntu
```

We can run a new container with the image and try to see the contents of the flag file:
```
student@localhost:~$ docker run ubuntuderived cat flag
Overwritten
```

Unfortunately, it has been overwritten!

Fortunately, Docker allows us to view the history of an image:
```
student@localhost:~$ docker history ubuntuderived
IMAGE               CREATED             CREATED BY                                      SIZE                COMMENT
1b0b3bfeee1b        18 months ago       /bin/sh -c chmod +x /bin/bash                   1.11MB              
<missing>           18 months ago       /bin/sh -c echo "Overwritten" > /root/flag      12B                 
<missing>           18 months ago       /bin/sh -c #(nop) WORKDIR /root                 0B                  
<missing>           18 months ago       /bin/sh -c md5sum /bin/bash > /root/flag        44B                 
<missing>           19 months ago       /bin/sh -c #(nop)  CMD ["/bin/bash"]            0B                  
<missing>           19 months ago       /bin/sh -c mkdir -p /run/systemd && echo 'do…   7B                  
<missing>           19 months ago       /bin/sh -c set -xe   && echo '#!/bin/sh' > /…   745B                
<missing>           19 months ago       /bin/sh -c [ -z "$(apt-get indextargets)" ]     987kB               
<missing>           19 months ago       /bin/sh -c #(nop) ADD file:a48a5dc1b9dbfc632…   63.2MB           
```

From the history, we can see that before the flag file was overwritten, it contained the MD5 hash of `/bin/bash`. Since `/bin/bash` has not been changed since then, to recover it, we can simply get its hash again:
```
student@localhost:~$ docker run ubuntuderived md5sum /bin/bash 
557c0271e30cf474e0f46f93721fd1ba  /bin/bash
```

### Flag:
```
557c0271e30cf474e0f46f93721fd1ba
```
