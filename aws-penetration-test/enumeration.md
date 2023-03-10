# Enumeration

## Enumeration

{% embed url="https://github.com/initstring/cloud_enum" %}

By giving the name of the company or organization it provides results related to that organization with AWS, Azure, and Google Cloud checks.

```
./cloud_enum.py -k <company> -k <company web site> -k <company product names>
```

{% embed url="https://github.com/darkbitio/aws-recon" %}

###

###

### IAM Enumeration

(Should be authenticated)

List of IAM Users :

```
aws iam list-users 
```

List the IAM groups that the specified IAM user belongs to :&#x20;

```
aws iam list-groups-for-user --user-name user-name
```

List all manages policies that are attached to the specified IAM user :&#x20;

```
aws iam list-attached-user-policies --user-name user-name
```

Lists the names of the inline policies embedded in the specified IAM user :&#x20;

```
aws iam list-user-policies --user-name user-name
```

Groups:

List of IAM Groups:

```
aws iam list-groups 
```

Lists all managed policies that are attached to the specified IAM Group :

```
aws iam list-attached-group-policies --group-name group-name
```

List the names of the inline policies embedded in the specified IAM Group:&#x20;

```
aws iam list-group-policies --group-name group-name
```

Roles:

List of IAM Roles :

```
aws iam list-roles 
```

Lists all managed policies that are attached to the specified IAM role :

```
aws iam list-attached-role-policies --role-name role-name
```

List the names of the inline policies embedded in the specified IAM role :&#x20;

```
aws iam list-role-policies --role-name role-name
```

Policies:

List of IAM Policies :

```
aws iam list-policies 
```

Retrieves information about the specified managed policy :&#x20;

```
aws iam get-policy --policy-arn policy-arn
```

Lists information about the versions of the specified manages policy :&#x20;

```
aws iam list-policy-versions --policy-arn policy-arn
```

Retrieved information about the specified version of the specified managed policy :&#x20;

```
aws iam get-policy-version --policy-arn policy-arn --version-id version-id
```

Retrieves the specified inline policy document that is embedded on the specified IAM user / group / role :

```
aws iam get-user-policy --user-name user-name--policy-name policy-name
```

```
aws iam get-group-policy --group-name group-name--policy-name policy-name
```

```
aws iam get-role-policy --role-name role-name--policy-name policy-name
```

### VPC Enumeration

Describe about VPCs :

```
aws ec2 describe-vpcs
```

Describe about Subnets :&#x20;

```
aws ec2 describe-subnets
```

Describe about Route Table  :

```
aws ec2 describe-route-tables
```

Describe about Network ACLs  :

```
aws ec2 describe-network-acls
```

### EC2 Enumeration

Describes the Information  about all Instances&#x20;

```
aws ec2 describe-instances 
```

Describes the Information  about Specified Instance&#x20;

```
aws ec2 describe-instances --instance-ids instance-id
```

Describes the Information about UserData Attribute of the specified Instance

```
aws ec2 describe-instance-attribute –attribute userData --instance-id instance-id
```

Describes the Information  about IAM instance profile associations&#x20;

```
aws ec2 describe-iam-instance-profile-associations
```

### EBS Enumeration

Describes the Information  about  EBS volumes&#x20;

```
aws ec2 describe-volumes 
```

Describes about all  the available EBS snapshots

```
aws ec2 describe-snapshots --owner-ids self
```

### Lambda Enumeration

During enumeration, we should do regions one by one because lambda functions region based. So if we enumerate without specifying the region it will look for the default region.

#### Lambda Function :

List of all the lambda functions &#x20;

```
aws lambda list-functions
```

Retrieves the Information about the specified lambda function  &#x20;

```
aws lambda get-function --function-name function-name
```

Retrieves the policy Information about the specified lambda function&#x20;

```
aws lambda get-policy --function-name function-name
```

Retrieves the event source mapping Information about the specified lambda function&#x20;

```
aws lambda list-event-source-mappings --function-name function-name
```

#### API Gateway:

List of all the Rest APIs&#x20;

```
aws apigateway get-rest-apis
```

Get the information about specified API

```
aws apigateway get-rest-api --rest-api-id ApiId
```

Lists information about a collection of resources

```
aws apigateway get-resources --rest-api-id ApiId
```

Get information about the specified resource

```
aws apigateway get-resource --rest-api-id ApiId --resource-id ResourceID
```

Get the method information for the specified resource &#x20;

```
aws apigateway get-method --rest-api-id ApiID--resource-id ResourceID--http-method Method
```

List of all stages for a REST API&#x20;

```
aws apigateway get-stages --rest-api-id ApiId
```

Get the information about the specified API's stage&#x20;

```
aws apigateway get-stage --rest-api-id ApiId--stage-name StageName
```

List of all the API keys&#x20;

```
aws apigateway get-api-keys --include-values
```

Get the information about a specified API key&#x20;

```
aws apigateway get-api-key --api-key ApiKey
```

### Containers Enumeration

Describe about  all the repositories in the container registry&#x20;

```
aws ecr describe-repositories
```

Get the information about repository policy&#x20;

```
aws ecr get-repository-policy --repository-name RepositoryName
```

Lists of all images in the specified repository&#x20;

```
aws ecr list-images  --repository-name RepositoryName
```

Describe the information about a container image&#x20;

```
aws ecr describe-images  --repository-name RepositoryName--image-ids imageTag=ImageTag
```

Lists all ECS Clusters&#x20;

```
aws ecs list-clusters
```

Describe information about specified cluster&#x20;

```
aws ecs describe-clusters --cluster ClusterName
```

Lists all services in the specified cluster

```
aws ecs list-services --cluster ClusterName
```

Describe the information about  a specified service&#x20;

```
aws ecs describe-services--cluster ClusterName--servicesServiceName
```

Lists all tasks in the specified cluster&#x20;

```
aws ecs list-tasks --cluster ClusterName
```

Describe the information about a specified task&#x20;

```
aws ecs describe-tasks  --cluster ClusterName--tasks TaskArn
```

Lists all containers in the specified cluster&#x20;

```
aws ecs list-container-instances --cluster Cluster-Name
```

Lists all EKS Clusters&#x20;

```
aws eks list-clusters
```

Describe the information about a specified cluster&#x20;

```
aws eks describe-cluster --name Cluster-Name
```

List of all node groups in a specified cluster

```
aws eks list-nodegroups--cluster-name Cluster-Name
```

Describe the information about a specific node group in a cluster&#x20;

```
aws eks describe-nodegroup--cluster-name Cluster-Name--nodegroup-name Node-Group
```

List of all fargate in a specified cluster&#x20;

```
aws eks list-fargate-profiles --cluster-name Cluster-Name
```

Describe the information about a specific fargate profile in a cluster&#x20;

```
aws eks describe-fargate-profile --cluster-name Cluster-Name--fargate-profile-name Profile-Name
```

### S3 Enumeration

List of all the buckets in the aws account &#x20;

```
aws s3api list-buckets
```

Get the information about specified bucket acls

```
aws s3api get-bucket-acl --bucket bucket-name
```

Get the information about specified bucket policy&#x20;

```
aws s3api get-bucket-policy --bucket bucket-name
```

Retrieves the Public Access Block configuration for an Amazon S3 bucket

```
aws s3api get-public-access-block --bucket bucket-name
```

List of all the objects in specified bucket &#x20;

```
aws s3api list-objects --bucket bucket-name
```

Get the aclsinformation about specified object  &#x20;

```
aws s3api get-object-acl--bucket bucket-name--key object-name
```

Searching s3 buckets online for any organization. :

https://buckets.grayhatwarfare.com/

### AWS RDS Enumeration

Describes the Information about the clusters in RDS

```
aws rds describe-db-clusters  
```

Describes the Information about the database instances in RDS

```
aws rds describe-db-instances 
```

Describes the Information about the subnet groups in RDS

```
aws rds describe-db-subnet-groups 
```

Describes the Information about  the database security groups in RDS

```
aws rds describe-db-security-groups 
```

Describes the Information about  the database proxies in RDS

```
aws rds describe-db-proxies 
```

### EBS Enumeration

Describes the Information  about  EBS volumes&#x20;

```
aws ec2 describe-volumes 
```

Describes about all  the available EBS snapshots

```
aws ec2 describe-snapshots --owner-ids self
```

### AWS Secret Manager Enumeration

Secret Manager

Lists of the all secrets that are stored by Secrets Manager

```
aws secretsmanager list-secrets 
```

Describes about specified secret

```
aws secretsmanager describe-secret --secret-id SecretName
```

Get the resource-based policy that is attached to the specified Secret

```
aws secretsmanager get-resource-policy --secret-id SecretID
```

Key Management Server

Lists of the all keys available in key management server (KMS)&#x20;

```
aws kms list-keys
```

Describes about specified key

```
aws kms describe-key--key-id KeyID
```

Lists of policies attached to specified key &#x20;

```
aws kms list-key-policies --key-id KeyID
```

Get full information about a policy &#x20;

```
aws kms get-key-policy --policy-name policy-name--key-id key-id
```

## Pacu

```
am__enum_users_roles_policies_groups
```

After that you can type

```
data
```

in order to see the results.

For enumeration in databases

```
run dynamodb__enum
```

Here is the list of all Pacu modules in order to do enumeration

```
[Category: ENUM]

  inspector__get_reports
  dynamodb__enum
  apigateway__enum
  iam__bruteforce_permissions
  rds__enum_snapshots
  cloudformation__download_data
  iam__detect_honeytokens
  iam__get_credential_report
  systemsmanager__download_parameters
  lambda__enum
  iam__enum_permissions
  rds__enum
  route53__enum
  acm__enum
  enum__secrets
  codebuild__enum
  ec2__download_userdata
  ecr__enum
  aws__enum_account
  ecs__enum_task_def
  iam__enum_users_roles_policies_groups
  aws__enum_spend
  ebs__enum_volumes_snapshots
  ec2__enum
  glue__enum
  eks__enum
  guardduty__list_accounts
  ecs__enum
  ec2__check_termination_protection
  transfer_family__enum
  lightsail__enum
  guardduty__list_findings
```
