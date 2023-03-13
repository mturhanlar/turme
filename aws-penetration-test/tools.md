# Tools

Some tools that you can use during your penetration test on AWS.&#x20;

### ScoutSuite

{% embed url="https://github.com/nccgroup/ScoutSuite" %}

### Pacu

{% embed url="https://github.com/RhinoSecurityLabs/pacu" %}

### Pmapper

{% embed url="https://github.com/nccgroup/PMapper" %}

#### Commands

Copy your env credentials and run these commands

```
export AWS_ACCESS_KEY_ID="zzzzzzzzz"
export AWS_SECRET_ACCESS_KEY="zzzzzzz"
export AWS_SESSION_TOKEN="zzzzzzzz"
```



This command will collect information from your&#x20;

```
pmapper graph create
```

List embedded queries

```
pmapper query list
```

Learn privilege escalation paths

```
pmapper --account "it_will_give_you_after_first_command" query -s 'preset privesc *'
```

Who can create user in resources.&#x20;

```
pmapper query 'who can do iam:CreateUser'
```

some refences:

* [https://github.com/nccgroup/PMapper/wiki/Query-Reference](https://github.com/nccgroup/PMapper/wiki/Query-Reference)&#x20;
* [https://securityonline.info/pmapper/?utm\_content=cmp-true](https://securityonline.info/pmapper/?utm\_content=cmp-true)
* [https://www.kitploit.com/2018/08/pmapper-tool-for-quickly-evaluating-iam.html?m=0](https://www.kitploit.com/2018/08/pmapper-tool-for-quickly-evaluating-iam.html?m=0)

## Links

[https://github.com/toniblyx/my-arsenal-of-aws-security-tools](https://github.com/toniblyx/my-arsenal-of-aws-security-tools)
