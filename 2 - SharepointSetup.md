# SharePoint install guide
```cmd
.\prerequisiteinstaller.exe
```
- After prerequisiteinstaller runs then run
```cmd
.\setup.exe
```
# Follow the steps lined out in the SharePoint Products Configuration Wizard to configure SharePoint to work with MIM.
- On the Connect to a server farm tab, change to create a new server farm.
- Specify this server as the database server like corpsql for the configuration database, and Contoso\SharePoint as the database access account for SharePoint to use.
- Create a password for the farm security passphrase.
- In the configuration Wizard we recommend selecting MinRole type of Front-end
- When the configuration wizard completes configuration task 10 of 10, click Finish and a web browser will open..
- If prompted the Internet Explorer popup, authenticate as Contoso\miminstall (or the equivalent administrator account) to proceed.
- In the web wizard (within the web app) click Cancel/Skip.

# Prepare SharePoint to host the MIM portal
- Launch Sharepoint management shell and run the following script to create the SharePoint 2016 Web Application
```PowerShell
New-SPManagedAccount ##Will prompt for new account enter contoso\mimpool 
$dbManagedAccount = Get-SPManagedAccount -Identity contoso\mimpool
New-SpWebApplication -Name "MIM Portal" -ApplicationPool "MIMAppPool" -ApplicationPoolAccount $dbManagedAccount -AuthenticationMethod "Kerberos" -Port 80 -URL http://mim.contoso.com
```
# Note
```
A warning message will appear saying that Windows Classic authentication method is being used, and it may take several minutes for the final command to return. 
When completed, the output will indicate the URL of the new portal. 
Keep the SharePoint 2016 Management Shell window open to reference later.
```
# Sharepoint Mangement Shell to create the SharePoint Site Collection
```PowerShell
$t = Get-SPWebTemplate -compatibilityLevel 15 -Identity "STS#1"
$w = Get-SPWebApplication http://mim.contoso.com/
New-SPSite -Url $w.Url -Template $t -OwnerAlias contoso\miminstall -CompatibilityLevel 15 -Name "MIM Portal"
$s = SpSite($w.Url)
$s.CompatibilityLevel
```
- CompatibilityLevel should return 15 as a result if not 15 the site collection needs to be created again
#SharePoint 2019 run this command to unblock ASHX files
```PowerShell
$w.BlockedASPNetExtensions.Remove("ashx")
$w.Update()
$w.BlockedASPNetExtensions
```
- Disable the SharePoint Server Side Viewstate and health task
```PowerShell
$contentService = [Microsoft.SharePoint.Administration.SPWebService]::ContentService;
$contentService.ViewStateOnServer = $false;
$contentService.Update();
Get-SPTimerJob hourly-all-sptimerservice-health-analysis-job | disable-SPTimerJob
```
# Add the SSL certificate
- Obtain an SSL PFX with the name of the server
- Import the PFX file in the web portal server
- Configure IIS SSL bindings to port 443 with the imported SSL create
# Configure SharePoint for SSL
- Open Central Admin on the SharePoint server
-- Open Application Management then select alertnate access mappings
-- Select the web app top right drop down menu
-- edit the default URL to https
