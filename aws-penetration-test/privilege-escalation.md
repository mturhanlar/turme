# Privilege Escalation


Pacu tool 
```
run iam__bruteforce_permissions
```

## IAM Privilege Escalation

Lists all managed policies that are attached to the specified IAM user :

```
aws iam list-attached-user-policies --user-name user-name
```

Retrieves information about the specified managed policy : 

```
aws iam get-policy --policy-arn policy-arn
```

Lists information about the versions of the specified managed policy :

```
aws iam list-policy-versions --policy-arn policy-arn
```

Retrieves information about the specified version of the specified managed policy :

```
aws iam get-policy-version --policy-arn policy-arn--version-id version-id
```

Add an inline policy document that is embedded in the specified IAM user :

```
aws iam put-user-policy --user-name Username--Policy-name PolicyName--policy-document file://Policy.json
```

Lists the names of the inline policies embedded in the specified IAM user :

```
aws iam list-user-policies --user-name user-name
```


[AWS IAM Privilege Escalation – Methods and Mitigation](https://rhinosecuritylabs.com/aws/aws-privilege-escalation-methods-mitigation/)

| **Privilege Escalation Methods **                          | **Required Permission **                                 |
|------------------------------------------------------------|----------------------------------------------------------|
| Attaching a policy to a user                               | iam:AttachUserPolicy                                     |
| Attaching a policy to a group                              | iam:AttachGroupPolicy                                    |
| Attaching a policy to a role                               | iam:AttachRolePolicy                                     |
| Creating a new user access key                             | iam:CreateAccessKey                                      |
| Creating a new login profile                               | iam:CreateLoginProfile                                   |
| Updating an existing login profile                         | iam:UpdateLoginProfile                                   |
| Creating an EC2 instance with an existing instance profile | iam:PassRole ec2:RunInstances                            |
| Creating/updating an inline policy for a user              | iam:PutUserPolicy                                        |
| Creating/updating an inline policy for a group             | iam:PutGroupPolicy                                       |
| Creating/updating an inline policy for a role              | iam:PutRolePolicy                                        |
| Adding a user to a group                                   | iam:AddUserToGroup                                       |
| Updating the AssumeRolePolicyDocumentof a role             | iam:UpdateAssumeRolePolicy sts:AssumeRole                |
| Passing a role to a new Lambda function, then invoking it  | iam:PassRole lambda:CreateFunction lambda:InvokeFunction |
| Updating the code of an existing Lambda function           | lambda:UpdateFunctionCode                                |



## EC2 Privilege Escalation

Get Information about user identity / role identity   :

```
aws sts get-caller-identity
```

Lists all managed policies that are attached to the specified IAM user :

```
aws iam list-attached-user-policies --user-name user-name
```

Retrieves information about the specified version of the specified managed policy :

```
aws iam get-policy-version --policy-arnpolicy-arn--version-id version-id
```

Get-Information about instance id  :

```
curl http://169.254.169.254/latest/meta-data/instance-id
```
Lists the instance profiles :

```
aws iam list-instance-profiles
```
Attach an instance profile with a role to a EC2 instance: : 

```
aws ec2 associate-iam-instance-profile --instance-id InstanceID --iam-instance-profile Name=ProfileName
```