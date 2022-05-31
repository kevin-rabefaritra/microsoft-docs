# [How To] Use the compliance search action to perform targetted email deletion
## Introduction
Compliance search action, commonly known as  content search, is often used to pull out data based on some criteria. However, instead of retrieving those items for download, matching items can be deleted using PowerShell.
This guide shows how to perform a targetted email deletion based on the email subject.

Custom criteria can be defined, you may refer to the [Keyword Query Language (KQL) syntax reference](https://docs.microsoft.com/en-us/sharepoint/dev/general-development/keyword-query-language-kql-syntax-reference).


## Procedure
To delete emails that match a specific email subject across all mailboxes.

1. [Connect to Exchange Online PowerShell](https://docs.microsoft.com/en-us/powershell/exchange/connect-to-exchange-online-powershell).

2. Connect to the Security & Compliance Center PowerShell.
```
Connect-IPPSSession
```

3. Create your compliance search instance. In this example, we will select all emails with the email subject "*Contoso Meeting*" and name our compliance search "ComplianceSearch_1".

**Note**: You can choose any name, this will only be used for internal reference.

```
New-ComplianceSearch -Name "ComplianceSearch_1" -ExchangeLocation All -ContentMatchQuery '(c:c)(subjecttitle="Contoso Meeting")'
```

4. Now that the compliance search instance has been created. We need to start it.

```
Start-ComplianceSearch "ComplianceSearch_1"
```

5. To track the compliance search progress, run:

```
Get-ComplianceSearch "ComplianceSearch_1" | Select-Object Status, JobProgress
```

You may need to perform this cmdlet multiple times until you see the search completed.

```
Status    JobProgress
------    -----------
Completed         100
```

6. To make sure that our search has found matching items, we can run:
```
Get-ComplianceSearch "ComplianceSearch_1" | FL SuccessResults
```

This shows the number of matching items per mailbox.

7. To perform a delete action on those matching items. At the same time, we store the instance to a variable `$complianceSearchAction` that will be used later.

```
$complianceSearchAction = New-ComplianceSearchAction -SearchName "ComplianceSearch_1" -Purge -PurgeType SoftDelete
```

**Note**: the `-PurgeType` parameter allows two values:

`SoftDelete` to allow the items to be recovered by the user.

`HardDelete` to have the items permanently deleted next time MFA (Managed Folder Assistant) is performed.

8. Track the search action progress with
```
$complianceSearchAction | Get-ComplianceSearchAction | Select-Object Status, JobProgress
```

Once the status shows as finished, the items should be deleted.

```
Status    JobProgress
------    -----------
Completed         100
```


## More information
[Search for and delete email messages in your organization | Microsoft Purview (compliance)](https://docs.microsoft.com/en-us/microsoft-365/compliance/search-for-and-delete-messages-in-your-organization?view=o365-worldwide)

[New-ComplianceSearch (Exchange PowerShell)](https://docs.microsoft.com/en-us/powershell/module/exchange/new-compliancesearch?view=exchange-ps)

[New-ComplianceSearchAction (Exchange PowerShell)](https://docs.microsoft.com/en-us/powershell/module/exchange/new-compliancesearchaction?view=exchange-ps)