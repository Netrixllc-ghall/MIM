# Accounts Needed Overview
# MIM Service Account (Can be gMSA)
- Runs the MIM Service
- No explicit permissions required
- Should have specific permissions stripped on MIM Servers to secure account

# MIM Sync Service Account (Can be gMSA)
- Runs the MIM Sync Service
- No explicit permissions required
- Should have specific permissions stripped on MIM Servers to secure account

# AD Management Agent (ADMA) Account 
- Used to connect to AD and sync to / from directory
- Requires permissions to do all sync actions (read users, write attributes, etc.)
- Requires Replicating Directory Changes, Replicating Directory Changes All
	
# MIM Management Agent (MIMMA) Account (Can be gMSA. If so, will use MIM Sync Svc Acct)
Used to connect to the MIM Database to sync to / from MIM Portal
Requires db_owner role on FIMService database

# MIM Service Installation Account 
- Used to install the MIM Service, becomes root administrative account
- Must be local admin on MIM Server (ONLY FOR INSTALLATION)
- Must have Sysadmin role in SQL (ONLY FOR INSTALLATION)
- Should have specific permissions stripped on MIM Servers to secure account
	
# MIM Task Execution Account
- Used to execute scheduled tasks to kick off synchronization jobs
- Must be a member of the MIMSyncAdmins group
- Should have specific permissions stripped on MIM Servers to secure account
	
# SharePoint Service Account
- Runs the SharePoint Service
- Standard User

# MIM Portal App Pool Account
- Runs the MIM Portal Application Pool
- Standard User

# Detailed Instructions
https://social.technet.microsoft.com/wiki/contents/articles/33214.fim-2010mim-2016-planning-security-setup-for-accounts-groups-and-services-part-4-detailed-description.aspx