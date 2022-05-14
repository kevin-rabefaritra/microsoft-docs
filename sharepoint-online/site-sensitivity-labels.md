# SharePoint sites / Microsoft Teams / Microsoft 365 groups sensitivity labels
## Introduction
By default, sensitivity labels are not available to be applied to entire SharePoint sites, Microsoft Teams, or Microsoft 365 groups.
To enable the capability, a set of PowerShell cmdlets need to be executed.

All of the PowerShell mentioned in this guide are from the Microsoft 365 Docs, referenced in the "More information" part at the bottom of this page. The intent of this guide is to simplify the steps and compile the articles into one.

![Sensitivity label for groups](res/groupsandsites-scope-options-sensitivity-label.png "Sensitivity label creation wizard for Group & sites")

## Procedure
1. Install, import, and connect to the Azure AD Preview module.

```
Install-Module AzureADPreview -Force -AllowClobber
Import-Module AzureADPreview
Connect-AzureAD
```

**Note**: the parameters `-Force` and `-AllowClobber` are important because some of the cmdlets in the AzureADPreview module are already included in the existing module AzureAD, thus creating a conflict.

2. Connect to the Security & Compliance module.
```
Install-Module ExchangeOnlineManagement
Import-Module ExchangeOnlineManagement
Connect-IPPSSession
```

3. Get the SettingsTemplate object that defines the usage guideline URL value.
```
$TemplateId = (Get-AzureADDirectorySettingTemplate | where { $_.DisplayName -eq "Group.Unified" }).Id
$Template = Get-AzureADDirectorySettingTemplate | where -Property Id -Value $TemplateId -EQ
```

4. Create a new settings object based on that template:
```
$Setting = $Template.CreateDirectorySetting()
```

5. Update the settings object with a new value. The two examples below change the usage guideline value and enable sensitivity labels.
```
$Setting["UsageGuidelinesUrl"] = "https://guideline.example.com"
$Setting["EnableMIPLabels"] = "True"
```

6. Then apply the setting:
```
New-AzureADDirectorySetting -DirectorySetting $Setting
```

7. Fetch the current group settings for the Azure AD organization.
```
$grpUnifiedSetting = (Get-AzureADDirectorySetting | where -Property DisplayName -Value "Group.Unified" -EQ)
$Setting = $grpUnifiedSetting
```

8. Enable the feature.
```
$Setting["EnableMIPLabels"] = "True"
```

9. Save the changes and apply the settings.
```
Set-AzureADDirectorySetting -Id $grpUnifiedSetting.Id -DirectorySetting $Setting
```

10. Synchronize your sensitivity labels to Azure AD.
```
Execute-AzureAdLabelSync
```

If the cmdlets performed above did not result in any error message. You should be able to see the "Groups & sites" option available when creating new sensitivity labels in the Microsoft 365 ~~Security & Compliance~~ Purview portal.


## More information
[Use sensitivity labels to protect content in Microsoft Teams, Microsoft 365 groups, and SharePoint sites | Microsoft Docs](https://docs.microsoft.com/en-us/microsoft-365/compliance/sensitivity-labels-teams-groups-sites?view=o365-worldwide)

[Assign sensitivity labels to Microsoft 365 groups in Azure Active Directory | Microsoft Docs](https://docs.microsoft.com/en-us/azure/active-directory/enterprise-users/groups-assign-sensitivity-labels)

[Azure Active Directory cmdlets for configuring group settings](https://docs.microsoft.com/en-us/azure/active-directory/enterprise-users/groups-settings-cmdlets)