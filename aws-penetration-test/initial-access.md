# Initial Access

## Initial Access

For initial access to AWS you should find access keys and tokens of an account. That can be done with some different ways phishing, vulnerable an application, leaked tokens/credentials etc.

There are some steps can be done during that phase.

## IAM Initial Access

Console sign in URL for root User :

https://signin.aws.amazon.com/console

Console sign in URL for IAM User :

https://account-ID-or-alias.signin.aws.amazon.com/console

Configure AWS CLI  :

```
aws configure --profile profile-name
```

### EC2 Privilege Escalation

Get Information about user identity / role identity   :

```
aws sts get-caller-identity
```

Lists all managed policies that are attached to the specified IAM user:

```
aws iam list-attached-user-policies --user-name user-name
```

Retrieves information about the specified version of the specified managed policy :

```
aws iam get-policy-version --policy-arnpolicy-arn--version-id version-id
```

Get-Information about instance id  :

```
curl http://169.254.169.254/latest/meta-data/instance-id
```

Lists the instance profiles :

```
aws iam list-instance-profiles
```

Attach an instance profile with a role to a EC2 instance: :&#x20;

```
aws ec2 associate-iam-instance-profile --instance-id InstanceID --iam-instance-profile Name=ProfileName
```
