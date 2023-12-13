# Tools

\*\*Recommended Modules to install:

* [ ] Az

```powershell
Install-Module -Name Az -Repository PSGallery -Force
```

* [ ] AzureAd

```powershell
Install-Module AzureAD
```

* [ ] MSOnline https://learn.microsoft.com/en-us/powershell/azure/active-directory/install-msonlinev1?view=azureadps-1.0

```powershell
Install-Module MSOnline
```

\*\*For everything main tool:

* [ ] AADInternals https://github.com/Gerenios/AADInternals

```powershell
Install-Module AADInternals
```

\*\*User Enumeration

* [ ] [https://github.com/LMGsec/o365creeper](https://github.com/LMGsec/o365creeper)

```bash
This script depends on the Python "Requests" library. The script can take a single email address
with the -e parameter or a list of email addresses, one per line, with the -f parameter. 
Additionally, the script can output valid email addressesto a file with the -o parameter.

Examples:
o365creeper.py -e test@example.com
o365creeper.py -f emails.txt
o365creeper.py -f emails.txt -o validemails.txt
```

\*\*If there is ADFS implemented we can use

Onedrive user enumeration

* [ ] https://github.com/nyxgeek/onedrive\_user\_enum

\*\*Password Spray:

* [ ] MSOLSpray https://github.com/dafthack/MSOLSpray

```powershell
Import-Module MSOLSpray.ps1
Invoke-MSOLSpray -UserList .\userlist.txt -Password Winter2020
```

* [ ] [https://github.com/Tw1sm/spraycharles](https://github.com/Tw1sm/spraycharles) spraycharles python low and slow password spraying tool

```bash
pip3 install pipx
pipx ensurepath
```

```bash
pipx install spraycharles
```

```
git clone https://github.com/Tw1sm/spraycharles
cd spraycharles/extras
docker build . -t spraycharles
```

```
docker run -it -v ~/.spraycharles:/root/.spraycharles spraycharles -h
```

* [ ] [https://github.com/CausticKirbyZ/SprayCannon](https://github.com/CausticKirbyZ/SprayCannon) More Better in Linux

Download releases from here

https://github.com/CausticKirbyZ/SprayCannon/releases

* [ ] \[https://github.com/blacklanternsecurity/TREVORspray] python tool

```bash
pip install git+https://github.com/blacklanternsecurity/trevorproxy
pip install git+https://github.com/blacklanternsecurity/trevorspray
```

* [ ] https://github.com/NetSPI/MicroBurst

```powershell
Import-Module .\MicroBurst.psm1
```

[https://github.com/NetSPI/MicroBurst/blob/master/Misc/Invoke-EnumerateAzureBlobs.ps1](https://github.com/NetSPI/MicroBurst/blob/master/Misc/Invoke-EnumerateAzureBlobs.ps1)

* [ ] https://github.com/nyxgeek/o365recon

```powershell
.\o365recon.ps1 -azure
```

* [ ] [https://github.com/Flangvik/TeamFiltration](https://github.com/Flangvik/TeamFiltration)

Just Download releases

https://github.com/Flangvik/TeamFiltration/releases

* [ ] https://github.com/rvrsh3ll/TokenTactics

```powershell
Import-Module .\TokenTactics.psd1
Get-Help Get-AzureToken
Invoke-RefreshToSubstrateToken
```

* [ ] https://github.com/dafthack/GraphRunner

```powershell
Import-Module .\GraphRunner.ps1
Get-GraphTokens
```

\*\*Security Auditing Tools for Azure AD:

* [ ] [https://github.com/nccgroup/ScoutSuite](https://github.com/nccgroup/ScoutSuite)
* [ ] [https://github.com/prowler-cloud/prowler](https://github.com/prowler-cloud/prowler)
* [ ] [https://github.com/Azure/Stormspotter](https://github.com/Azure/Stormspotter) Azure Red Team tool but need to configure it. Python
* [ ] [https://github.com/AzureAD/AzureADAssessment](https://github.com/AzureAD/AzureADAssessment)
* [ ] [https://github.com/vletoux/pingcastle](https://github.com/vletoux/pingcastle) Azure AD Assessment Tool
* [ ] https://github.com/dirkjanm/ROADtools

References: https://github.com/Gerenios/AADInternals [https://github.com/LMGsec/o365creeper](https://github.com/LMGsec/o365creeper) https://github.com/dafthack/MSOLSpray https://github.com/NetSPI/MicroBurst https://github.com/nyxgeek/o365recon [https://github.com/Tw1sm/spraycharles](https://github.com/Tw1sm/spraycharles) [https://github.com/CausticKirbyZ/SprayCannon](https://github.com/CausticKirbyZ/SprayCannon) \[https://github.com/blacklanternsecurity/TREVORspray] (https://github.com/blacklanternsecurity/TREVORspray) [https://github.com/Flangvik/TeamFiltration](https://github.com/Flangvik/TeamFiltration) https://github.com/rvrsh3ll/TokenTactics [https://github.com/NetSPI/MicroBurst/blob/master/Misc/Invoke-EnumerateAzureBlobs.ps1](https://github.com/NetSPI/MicroBurst/blob/master/Misc/Invoke-EnumerateAzureBlobs.ps1) https://github.com/dafthack/GraphRunner [https://github.com/nccgroup/ScoutSuite](https://github.com/nccgroup/ScoutSuite) [https://github.com/prowler-cloud/prowler](https://github.com/prowler-cloud/prowler) [https://github.com/Azure/Stormspotter](https://github.com/Azure/Stormspotter) [https://github.com/AzureAD/AzureADAssessment](https://github.com/AzureAD/AzureADAssessment) [https://github.com/vletoux/pingcastle](https://github.com/vletoux/pingcastle) https://github.com/dirkjanm/ROADtoolsa
