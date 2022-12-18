# Enumeration

Enumeration

Install AADInternals

```
Set-ExecutionPolicy Unrestricted
```
Install the module

```
Install-Module AADInternals
```

Import the module

```
Import-Module AADInternals
```

Get tenant name, authentication, brand name (usually same as directory name) and domain name

```
Get-AADIntLoginInformation -UserName unsecure@yourdomain.com
```

Get tenant ID

```
Get-AADIntTenantID -Domain yourdomain.com 
```

Get tenant domains

```
Get-AADIntTenantDomains -Domain yourdomain.com 
```


Get all the information

```
Invoke-AADIntReconAsOutsider -DomainName  
```

ROAD Tool

ROADtools is a framework to interact with Azure AD. It consists of a library (roadlib) with common components, the ROADrecon Azure AD exploration tool and the ROADtools Token eXchange (roadtx) tool.

install it via pip

```
roadrecon auth -u [MAIL] -p [PASSWORD]
roadrecon gather
```

later

```
roadrecon gui
```

AzureAD Module

AzureAD is a PowerShell module from Microsoft for managing Azure AD.

Can be used only to interact with Azure AD, no access to Azure resources.

```
Install-Module AzureADcommand (needs internet)

Import-Module AzureAD

$creds=Get-Credential

Connect-AzureAD -Credential $creds 

$passwd=ConvertTo-SecureString"[PASSWORD]" -AsPlainText -Force

$creds=New-ObjectSystem.Management.Automation.PSCredential("[DOMAIN]",$passwd) 

Connect-AzureAD -Credential $creds
``` 

Get the current session state

```
Get-AzureADCurrentSessionInfo
```

Get details of the current tenant

```
Get-AzureADTenantDetail
```


AzureAD Users

Enumerate all users

```
Get-AzureADUser -All $true
```

Enumerate a specific user

```
Get-AzureADUser-ObjectId [MAIL]
Get-AzureADUser -SearchString "admin" 
```

Search for users who contain the word "admin" in their Display name:
```
Get-AzureADUser-All $true|?{$_.Displayname -match "admin"}
```
List all the attributes for a user

```
Get-AzureADUser-ObjectId [MAIL] | fl * 

Get-AzureADUser-ObjectId [MAIL] |%{$_.PSObject.Properties.Name}
```

Search attributes for all users that contain the string "password":

```
Get-AzureADUser -All $true |%{$Properties=$;$Properties.PSObject.Properties.Name|%{if($Properties.$-match'password') {"$($Properties.UserPrincipalName)-$-$($Properties.$)"}}}
```
All users who are synced from on-prem

```
Get-AzureADUser -All $true |?{$_.OnPremisesSecurityIdentifier -ne $null}
```

All users who are from Azure AD

```
Get-AzureADUser -All $true |?{$_.OnPremisesSecurityIdentifier -eq $null}
```

Objects created by any user (use -ObjectIdfor a specific user)

```
Get-AzureADUser | Get-AzureADUserCreatedObject
```
Objects owned by a specific user

```
Get-AzureADUserOwnedObject -ObjectId [MAIL]
```

AzureAD Groups

List all Groups

```
Get-AzureADGroup -All $true
```

Enumerate a specific group

```
Get-AzureADGroup -ObjectId .....-..... 
```

Search for a groupbased on string in first characters of DisplayName(wildcard not supported)

```
Get-AzureADGroup -SearchString "admin" | fl * 
```
To search for groups which contain the word "admin" in their name:

```
Get-AzureADGroup -All $true |?{$_.Displayname-match"admin"}
```

Get Groups that allow Dynamic membership (Note the cmdlet name)

```
Get-AzureADMSGroup |?{$_.GroupTypes -eq 'DynamicMembership'}
```

All groups that are synced from on-prem(note that security groups are not synced)

```
Get-AzureADGroup -All $true |?{$_.OnPremisesSecurityIdentifier -ne $null} 
```

All groups that are from Azure AD

```
Get-AzureADGroup -All $true |?{$_.OnPremisesSecurityIdentifier -eq $null}
```

Get members of a group

```
Get-AzureADGroupMember -ObjectId ........-....-....-....-................. 
```

Get groups and roles where the specified user is a member

```
Get-AzureADUser-SearchString'test'|Get-AzureADUserMembership
Get-AzureADUserMembership -ObjectId unsecure@yourdomain.com

AzureAD Role

Get all available role templates

```
Get-AzureADDirectoryroleTemplate
```

Get all roles
```
Get-AzureADDirectoryRole
```

Enumerate users to whom roles are assigned

Enumerating Admin Roles in AzureAD

```
Get-AzureADDirectoryRole -Filter "DisplayName eq 'Global Administrator'" | Get-AzureADDirectoryRoleMember
```

AzureAD Devices

Get all Azure joined and registered devices

```
Get-AzureADDevice -All $true | fl * 
```

Get the device configuration object (note the RegistrationQuotain the output)

```
Get-AzureADDeviceConfiguration | fl *
```

List Registered owners of all the devices

```
Get-AzureADDevice -All $true |Get-AzureADDeviceRegisteredOwner
```

List Registered users of all the devices

```
Get-AzureADDevice -All $true | Get-AzureADDeviceRegisteredUser
```

List devices owned by a user

```
Get-AzureADUserOwnedDevice -ObjectId [MAIL] 
```

List devices registered by a user

```
Get-AzureADUserRegisteredDevice -ObjectId [MAIL]  
```

List devices managed using Intune

```
Get-AzureADDevice -All $true |?{$_.IsCompliant-eq"True"}
```

AzureAD Apps

Get all the application objects registered with the current tenant (visible in App Registrations in Azure portal). An application object is the global representation of an app.


```
Get-AzureADApplication -All $true
```


Get all details about an application


```
Get-AzureADApplication -ObjectId [ID NO]  | fl *
```

Get an application based on the display name

```
Get-AzureADApplication -All $true | ?{$_.DisplayName-match"app"}
```


The  Get-AzureADApplicationPasswordCredential will show the applications with an application password but the password value is not shown.

Get the owner of an application

```
Get-AzureADApplication -ObjectId [ID NO] | Get-AzureADApplicationOwner | fl * 
```

Get Apps where a User has a role (exact role is not shown)

```
Get-AzureADUser-ObjectId [MAIL] |Get-AzureADUserAppRoleAssignment | fl *
```

Get Apps where a Group has a role (exact role is not shown)

```
Get-AzureADGroup -ObjectId [ID NO] | Get-AzureADGroupAppRoleAssignment | fl *
```

AzureAD Service Principals

Enumerate Service Principals (visible as Enterprise Applications in Azure Portal).

The service principal is a local representation for an app in a specific tenant and it is the security object that has privileges. This is the 'service account'! Service Principals can be assigned Azure roles.

Get all service principals

```
Get-AzureADServicePrincipal -All $true
```

Get all details about a service principal

```
Get-AzureADServicePrincipal -ObjectId [ID NO]  | fl *
```

Get a service principal based on the display name

```
Get-AzureADServicePrincipal-All$true|?{$_.DisplayName-match"app"}
```

Get the owner of a service principal

```
Get-AzureADServicePrincipal-ObjectId [ID no] | Get-AzureADServicePrincipalOwner | fl *
```

Get objects owned by a service principal

```
Get-AzureADServicePrincipal-ObjectId [ID no] | Get-AzureADServicePrincipalOwnedObject
```

Get objects created by a service principal

```
Get-AzureADServicePrincipal-ObjectId cdddd16e-2611-4442-8f45-234rwf234 | Get-AzureADServicePrincipalCreatedObject
```

Get group and role memberships of a service principal

```
Get-AzureADServicePrincipal -ObjectId [ID no] | Get-AzureADServicePrincipalMembership | fl *
```

```
Get-AzureADServicePrincipal | Get-AzureADServicePrincipalMembership
```

