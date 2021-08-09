# Notes
- As a general rule, in most cases when using a MIM installer, to specify that you want to use a gMSA instead of a regular account, append a dollar sign character to gMSA name, e.g. contoso\MIMSyncGMSAsvc$, 
and leave the password field empty. One exception is the miisactivate.exe tool that accepts gMSA name without the dollar sign.
# Create the necessary service accounts for MIM
```PowerShell
import-module activedirectory
$sp = ConvertTo-SecureString "Pass@word1" –asplaintext –force

New-ADUser –SamAccountName MIMAdmin –name MIMAdmin
Set-ADAccountPassword –identity MIMAdmin –NewPassword $sp
Set-ADUser –identity MIMAdmin –Enabled 1 –PasswordNeverExpires 1

New-ADUser –SamAccountName svcSharePoint –name svcSharePoint
Set-ADAccountPassword –identity svcSharePoint –NewPassword $sp
Set-ADUser –identity svcSharePoint –Enabled 1 –PasswordNeverExpires 1

New-ADUser –SamAccountName svcMIMSql –name svcMIMSql
Set-ADAccountPassword –identity svcMIMSql –NewPassword $sp
Set-ADUser –identity svcMIMSql –Enabled 1 –PasswordNeverExpires 1

New-ADUser –SamAccountName svcMIMAppPool –name svcMIMAppPool
Set-ADAccountPassword –identity svcMIMAppPool –NewPassword $sp
Set-ADUser –identity svcMIMAppPool –Enabled 1 -PasswordNeverExpires 1
```
# Create the security groups and add the members
```PowerShell
New-ADGroup –name MIMSyncAdmins –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncAdmins
New-ADGroup –name MIMSyncOperators –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncOperators
New-ADGroup –name MIMSyncJoiners –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncJoiners
New-ADGroup –name MIMSyncBrowse –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncBrowse
New-ADGroup –name MIMSyncPasswordSet –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncPasswordSet
Add-ADGroupMember -identity MIMSyncAdmins -Members Administrator
Add-ADGroupMember -identity MIMSyncAdmins -Members MIMAdmin
```
# Create the SPN for Kerberos Auth
```PowerShell
setspn -S http/mim.contoso.com contoso\svcMIMAppPool
```
# Create the Sync service account, the security group and the service principal spn
```PowerShell
New-ADGroup –name MIMSync_Servers –GroupCategory Security –GroupScope Global –SamAccountName MIMSync_Servers
Add-ADGroupmember -identity MIMSync_Servers -Members MIMSync$
```
# Create the gMSA for the Sync Servers
```PowerShell
New-ADServiceAccount -Name MIMSyncGMSAsvc -DNSHostName MIMSyncGMSAsvc.contoso.com -PrincipalsAllowedToRetrieveManagedPassword "MIMSync_Servers"
```
# Create the gMSA for the Service Servers
```PowerShell
New-ADServiceAccount -Name MIMSrvGMSAsvc -DNSHostName MIMSrvGMSAsvc.contoso.com -PrincipalsAllowedToRetrieveManagedPassword "MIMService_Servers" -OtherAttributes @{'msDS-AllowedToDelegateTo'='FIMService/mimportal.contoso.com'}
```
# Deletate SPN to gMSA
```PowerShell
Set-ADServiceAccount -Identity MIMSrvGMSAsvc -TrustedForDelegation $true -ServicePrincipalNames @{Add="FIMService/mimportal.contoso.com"}
```
# Optional - Passsword change service account is needed if you are going to allow users to use SSPR
```PowerShell
Set-ADServiceAccount -Identity MIMSyncGMSAsvc -ServicePrincipalNames @{Add="PCNSCLNT/mimsync.contoso.com"}
```
# Use MIM Synchronization Service group managed service account and do not create a separate account
```
You can skip creation of the MIM Service Management Agent service account. In this case, use MIM Synchronization Service gMSA name, e.g. contoso\MIMSyncGMSAsvc$, 
instead of the MIM MA account when installing MIM Service. Later on in the MIM Service Management Agent configuration enable 'Use MIMSync Account' option.
```
# Create the MIMservers AD Group
``` PowerShell
New-ADGroup –name MIMService_Servers –GroupCategory Security –GroupScope Global –SamAccountName MIMService_Servers
Add-ADGroupMember -identity MIMService_Servers -Members MIMPortal$
```

# Self service password reset needs to have Password reset rights deletated to the MIMSrv gMSA
```PowerShell
Add-ADGroupmember -identity MIMSyncPasswordSet -Members MIMSrvGMSAsvc$ 
Add-ADGroupmember -identity MIMSyncBrowse -Members MIMSrvGMSAsvc$
```
- Reboot your MIM Service server to refresh a Kerberos token associated with the server as the "MIMService_Servers" group membership has changed.







































