# Mailbox audit logs
## Introduction
By default, auditing is already enabled for all new mailboxes. But not every action performed on the mailbox is audited.

A very important non-audited activity is the **Move** action when running the `Search-MailboxAuditLog` cmdlet. This needs to be added manually through PowerShell.

## Procedure
To add **Move** and **Copy** to auditing.

1. [Connect to Exchange Online PowerShell](https://docs.microsoft.com/en-us/powershell/exchange/connect-to-exchange-online-powershell).

2. Run the following cmdlet to add for a specific mailbox
```
Set-Mailbox -Identity kevin@contoso.com -AuditOwner @{Add="Move"} -AuditDelegate @{Add="Move"} -AuditAdmin @{Add="Move"}
```

or run the following cmdlet to add for all mailboxes
```
Get-Mailbox -ResultSize Unlimited | Foreach {
    Set-Mailbox -Identity $_.PrimarySmtpAddress -AuditOwner @{Add="Move"} -AuditDelegate @{Add="Move"} -AuditAdmin @{Add="Move"}
}
```

Optionally, you can also add the **Copy** activity for administrators operations.
```
Set-Mailbox -Identity kevin@contoso.com -AuditOwner @{Add="Move"} -AuditDelegate @{Add="Move"} -AuditAdmin @{Add="Move", "Copy"}
```

You can also run the following command to verify which activities are audited for a mailbox:
```
$mailbox = Get-Mailbox kevin@menja.cc
$mailbox | Select-Object -Expand AuditAdmin
$mailbox | Select-Object -Expand AuditDelegate
$mailbox | Select-Object -Expand AuditOwner
```

## Analyzing the audit logs
While running the `Search-MailboxAuditLogs`, you will come across multiple activities. Below are each description per action.

| Action | Description |
| ------ | ------------- |
| Copy | An item is copied to another folder. |
| Create | An item is created in the Calendar, Contacts, Notes, or Tasks folder in the mailbox; for example, a new meeting request is created. Note that message or folder creation isn't audited. |
| FolderBind | A mailbox folder is accessed. |
| HardDelete | An item is deleted permanently from the Recoverable Items folder. |
| MailboxLogin | The user signed in to their mailbox. |
| MessageBind | An item is accessed in the reading pane or opened. |
| Move | An item is moved to another folder. |
| MoveToDeletedItems | An item is moved to the Deleted Items folder. |
| SendAs | A message is sent using Send As permissions. |
| SendOnBehalf | A message is sent using Send on Behalf permissions. |
| SoftDelete | An item is deleted from the Deleted Items folder. |
| Update | An item's properties are updated. |

## More information
[Manage mailbox auditing - Microsoft Purview | Microsoft Docs](https://docs.microsoft.com/en-us/microsoft-365/compliance/enable-mailbox-auditing?view=o365-worldwide)

[Mailbox audit logging in Exchange Server | Microsoft Docs](https://docs.microsoft.com/en-us/exchange/policy-and-compliance/mailbox-audit-logging/mailbox-audit-logging?view=exchserver-2019)

[Search-MailboxAuditLog (Exchange PowerShell) | Microsoft Docs](https://docs.microsoft.com/en-us/powershell/module/exchange/search-mailboxauditlog?view=exchange-ps)