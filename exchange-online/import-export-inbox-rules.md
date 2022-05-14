# [Draft] Import-export inbox rules
## Introduction
Inbox rules can be imported and exported as `.mzd` files using the Outlook desktop client. This page provides two PowerShell scripts for respectively exporting and importing inbox rules.

## Procedure
1. [Connect to Exchange Online PowerShell](https://docs.microsoft.com/en-us/powershell/exchange/connect-to-exchange-online-powershell).

2. To export the inbox rules of a specific mailbox.

```
Get-InboxRules 
```

## More information
[Import or export a set of rules | Microsoft Support](https://support.microsoft.com/en-us/office/import-or-export-a-set-of-rules-f54b5bd2-40e0-426e-9f25-e51fa14eeb95)
