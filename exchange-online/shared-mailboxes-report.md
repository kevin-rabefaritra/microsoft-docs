# Shared mailboxes report
## Introduction
This article discusses how to get a report for shared mailboxes delegations in your organization.

## Procedure
To export the shared mailboxes with SendAs / FullAccess permissions.

1. [Connect to Exchange Online PowerShell](https://docs.microsoft.com/en-us/powershell/exchange/connect-to-exchange-online-powershell).

2. To export the shared mailboxes delegation, run the following command:

```
$mailboxes = Get-Mailbox -RecipientTypeDetails SharedMailbox -ResultSize Unlimited
$mailboxes | ForEach-Object {
    $displayName = $_.DisplayName
    $membersAccess = ''
    $membersSendAs = ''
    Foreach($member in Get-MailboxPermission $_.PrimarySmtpAddress -Resultsize unlimited) {
        if($member.User -notlike '*NT AUTHORITY\SELF*') {
            $membersAccess = $membersAccess + $member.User + ";"
        }
    }

    Foreach($member in Get-RecipientPermission $_.PrimarySmtpAddress -AccessRights SendAs -Resultsize Unlimited) {
        if($member.Trustee -notlike '*NT AUTHORITY\SELF*') {
            $membersSendAs = $membersSendAs + $member.Trustee + ";"
        }
    }

    New-Object -TypeName PSObject -Property @{
        SharedMailboxName = $displayName
        EmailAddress = $_.PrimarySmtpAddress
        ReadAndManage = $membersAccess
        SendAs = $membersSendAs
    }
} | Export-CSV "Shared-Mailbox-Permissions.csv" -NoTypeInformation -Encoding UTF8
```

## More information
[Get-RecipientPermission (Exchange Online PowerShell) | Microsoft Docs](https://docs.microsoft.com/en-us/powershell/module/exchange/get-recipientpermission?view=exchange-ps)