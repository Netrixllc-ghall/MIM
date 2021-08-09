# Create the MIM management agent
The MIM management agent (MA) is a connector for MIM Sync to the MIM Service. To create this connector, you use the Create Management Agent wizard.
When you configure a MIM management agent, you need to specify a user account. This document uses MIMMA as the name for this account.

 Note

The account you use for your MIM management agent must be the same account as the one you have specified during the installation of MIM Service. If you plan to enable 'Use MIMSync account' feature then MIM Synchronization Service must be installed using Group Managed Service Account.

# To create the MIM MA
- Open the Synchronization Service Manager.
- To open the Create Management Agent wizard, change to the Management Agents page, then, on the Actions menu, click Create.
- On the Create Management Agent page, provide the following settings, and then click Next.

# Management agent for: FIM Service management agent
- Name: MIMMA

# On the Connect to Database page, provide the following settings, and then click Next
- Server: localhost
- Database: FIMService
- MIM Service base address: http://localhost:5725
- Authentication mode: Windows integrated authentication
- User name: mimma
- Password: Pass@word
- Domain: contoso
# On the Selected Object Types page, verify that the object types that are listed below are selected, and then click Next
- DetectedRuleEntry
- ExpectedRuleEntry
- Group
- Person
- SynchronizationRule
# On the Selected Attributes page, check Show All and verify that all listed attributes are selected, and then click Next.
- On the Configure Connector Filter page, click Next.
- On the Configure Object Type Mappings page, add the following mapping, and then click Next
- Select Person in the Data Source Object Type list.
- Click Add Mapping to open the Mapping dialog box.
- Select person in the Metaverse object type list.
- Click OK to close the Mapping dialog box.
- Select Group in the Data Source Object Type list.
- Click Add Mapping to open the Mapping dialog box.
- Select group in the Metaverse object type list.
- Click OK to close the Mapping dialog box.
- On the Configure Attribute Flow page, create attribute flow mappings as shown below, and then click Next
- Select Person as the Data source and Metaverse object types.
- Select Direct as the Mapping Type.
- For each row in the following table, complete these steps:
```
Select the Flow direction shown for that row in the table.
Select the Data source attribute shown for that row in the table.
Select the Metaverse attribute shown for that row in the table.
To apply the flow mapping, click New.
```
| Data Source Attribute | Flow Direction | Metaverse Attribute |
|-----------------------|----------------|---------------------|
| AccountName           | Export         | accountName         |
| DisplayName           | Export         | displayName         |
| Domain                | Export         | domain              |
| Email                 | Export         | mail                |
| EmployeeID            | Export         | employeeID          |
| EmployeeType          | Export         | employeeType        |
| FirstName             | Export         | firstName           |
| LastName              | Export         | lastName            |
| ObjectSID             | Export         | objectSid           |
- Select Group as the Data source type and Metaverse object types.
- Select Direct as the Mapping Type.
- For each row in the following table, complete these steps:
- Select the Flow direction shown for that row in the table.
- Select the Data source attribute shown for that row in the table.
- Select the Metaverse attribute shown for that row in the table.
- To apply the flow mapping, click New.

| Data Source Attribute | Flow Direction | Metaverse Attribute   |
|-----------------------|----------------|-----------------------|
| AccountName           | Export         | accountName           |
| DisplayName           | Export         | displayName           |
| Domain                | Export         | domain                |
| Email                 | Export         | mail                  |
| MailNickName          | Export         | mailNickName          |
| Member                | Export         | member                |
| ObjectSID             | Export         | objectSid             |
| Scope                 | Export         | scope                 |
| Type                  | Export         | type                  |
| MembershipAddWorkflow | Export         | membershipAddWorkflow |
| MembershipLocked      | Export         | membershipLocked      |
| AccountName           | Import         | accountName           |
| DisplayedOwner        | Import         | displayedOwner        |
| DisplayName           | Import         | displayName           |
| MailNickName          | Import         | mailNickName          |
| Member                | Import         | member                |
| Scope                 | Import         | scope                 |
| Type                  | Import         | type                  |

- On the Configure Deprovisioning page, click Next
- To create the management agent, on the Configure Extensions page, click Finish.
# Create the AD management agent
- The Active Directory management agent is a connector for AD Domain Services. To create this connector, you use the Create Management Agent wizard.
- To open the Create Management Agent wizard, on the Actions menu, click Create.
- On the Create Management Agent page, provide the following settings, and then click Next:
- Management agent for: Active Directory Domain Services
```
Name: ADMA
```
- On the Connect to Active Directory Forest page, provide the following settings, and then click Next:
```
Forest name: contoso.local
User name: administrator
Password : <the account’s password>
Domain: contoso
```
-On the Configure Directory Partitions page, provide the following settings, and then click Next:
- In the Select directory partitions list, select DC=CONTOSO, DC=local.
- To open the Select Containers dialog box, click Containers.
- If you wish to change the container to only have MIM manage objects in a particular container, click the DC=CONTOSO,DC=local node, and then click the node for the container of interest.
- To close the Select Containers dialog box, click OK.
- On the Configure Provisioning Hierarchy page, click Next.
- On the Select Object Types page, provide the following settings, and then click Next:
- In the Object types list, select user and group.
- On the Select Attributes page, check Show ALL, elect the following attributes, and then click Next:
```
company
displayName
employeeID
employeeType
givenName
groupType
managedBy
manager
member
objectSid
sAMAccountName
sAMAccountType
sn
unicodePwd
userAccountControl
```
- On the Configure Connector Filter page, click Next.
- On the Configure Join and Projection Rules page, click Next.
- On the Configure Attribute Flow page, click Next.
- On the Configure Deprovisioning page, click Next.
- On the Configure Extensions page, click Finish.
# Create Run Profiles
- Create run profiles for the ADMA and MIMMA Connectors.
- Create run profiles for the ADMA connector
# This table shows the five run profiles you will create for the ADMA connector:
| Name     | Type                      |
|----------|---------------------------|
| Profile1 | Full Import (Stage Only)  |
| Profile2 | Full Synchronization      |
| Profile3 | Delta Import (Stage Only) |
| Profile4 | Delta Synchronization     |
| Profile5 | Export                    |
# To create run profiles for the ADMA connector:
- Open the Synchronization Service Manager and on the Tools menu, click Management Agents.
- In the Management Agents list, select ADMA.
- To open the Configure Run Profiles for dialog box, on the Actions menu, click Configure Run Profiles.
- For each run profile in the table, complete the following steps:
- To open the Configure Run Profile wizard, click New Profile.
- In the Name box, type the profile name from the table, and click Next.
- In the Type list, select the step type from the table, and then click Next.
- Click Finish to create the run profile.
- To close the Configure Run Profiles dialog box, click OK.
# Create run profiles for the MIMMA connector
- This table shows the matching five run profiles for the MIMMA connector:
# CREATE RUN PROFILES FOR THE MIMMA CONNECTOR

| Name     | Type                      |
|----------|---------------------------|
| Profile1 | Full Import (Stage Only)  |
| Profile2 | Full Synchronization      |
| Profile3 | Delta Import (Stage Only) |
| Profile4 | Delta Synchronization     |
| Profile5 | Export                    |
# To create run profiles for the MIMMA connector:
- Open the Synchronization Service Manager, on the Tools menu, click Management Agents.
- In the Management Agents list, select MIMMA.
- To open the Configure Run Profiles for dialog box, on the Actions menu, click Configure Run Profiles.
# For each run profile in the table, complete the following steps:
- To open the Configure Run Profile wizard, click New Profile.
- In the Name box, type the profile name from the table, and click Next.
- In the Type list, select the step type from the table, and then click Next.
- Click Finish to create the run profile.
- To close the Configure Run Profiles dialog box, click OK.
# Configure the MIM Service
- Using the MIM Portal, you will create the AD user inbound synchronization rule for MIM Service.
## To create the AD user inbound synchronization rule:
- On the MIM Portal home page, on the navigation bar, click Administration.
- To open the Synchronization Rules page, click Synchronization Rules.
- To open the Create Synchronization Rule wizard, in the toolbar, click New.
- On the General tab, provide the following information, and then click Next:
```
Display Name: AD User Inbound Synchronization Rule
Data Flow Direction: Inbound
```
- On the Scope tab, provide the following information, and then click Next:
```
Metaverse Resource Type: person
External System: ADMA
External System Resource Type: user
```
- On the Relationship tab, provide the following information, and then click Next:
- To configure the Relationship Criteria, 
- select ObjectSID from the MetaverseObject:person(Attribute) list and from the ConnectedSystemObject:person(Attribute)list.
- Select Create Resource in FIM.
# On the Inbound Attribute Flow page, provide the following information, and then click Next:
| Flow Rule | Source         | Destination  |
|-----------|----------------|--------------|
| Rule 1    | samAccountName | accountName  |
| Rule 2    | displayName    | displayName  |
| Rule 3    | EmployeeType   | employeeType |
| Rule 4    | givenName      | firstName    |
| Rule 5    | sn             | lastName     |
| Rule 6    | Manager        | manager      |
| Rule 7    | objectSID      | ObjectSID    |
| Rule 8    | "Contoso"      | domain       |
For each row in this table, perform the following steps:
- To open the Flow Definition dialog box, click New Attribute Flow.
- On the Source tab, select the attribute shown for that row in the table.
- On the Destination tab, select the attribute shown for that row in the table.
- To apply the attribute flow configuration, click OK.
- On the Summary tab, click Submit.
# There are four steps you need to take before you can test your MIM configuration with AD data:
## Enable Provisioning
### Open the Synchronization Service Manager.
- To open the Options dialog box, on the Tools menu, click Options
- Select Enable Synchronization Rule Provisioning.
- To close the Options dialog box, click OK.
# Initialize the MIMMA
- Run a complete synchronization cycle on this connector. The complete cycle consists of the following run profiles:
```
Full Import
Full Synchronization
Export
Delta Import
```
# Follow these steps to run each of the four run profiles.
- Open the Synchronization Service Manager and, on the Tools menu, click Management Agents.
- In the Management Agents list, select MIMMA.
- To open the Run Management Agent dialog box, on the Actions menu, click Run.
- For each run profile listed above, complete the following steps:
- To open the Run Management Agent dialog box, on the Actions menu, click Run.
- In the Run profiles list, select the run profile you want to run.
- To start the run profile, click OK.

# Configure attribute flow precedence
During the initialization of the MIM connector, the configured synchronization rules were brought into the metaverse.
Adjust the attribute flow precedence for the attributes contributed by this connector to ensure that attributes already in AD can flow into the metaverse and later also into the MIM Service database.
# Initialize the ADMA
## To initialize the Active Directory connector, you need to run a full import and a full synchronization on it. The full import brings the existing objects from AD into the connector space. The full synchronization updates the synchronization rules to match those of the MIM connector.
- Open the Synchronization Service Manager and in the Tools menu, click Management Agents.
- In the Management Agents list, select ADMA.
- To open the Run Management Agent dialog box, on the Actions menu, click Run.
- For each run profile listed above, complete the following steps:
- To open the Run Management Agent dialog box, on the Actions menu, click Run.
- In the Run profiles list, select the run profile you want to run.
- To start the run profile, click OK.
# Populate the MIM Service database
## To populate the MIM Service database with the objects, you need to run a synchronization cycle on the MIMMA connector. The cycle consists of:
```
Export
Full Import
Full Synchronization
```
## Follow these steps to run each of the three run profiles.
- Open the Synchronization Service Manager and click Management Agents on the Tools menu.
- Select MIMMA in the Management Agents list.
- Click Run on the Actions menu to open the Run Management Agent dialog box.
## For each run profile listed above, complete the following steps:
Click Run on the Actions menu to open the Run Management Agent dialog box.
Select the run profile you want to run from the Run profiles list.
Click OK to start the run profile.