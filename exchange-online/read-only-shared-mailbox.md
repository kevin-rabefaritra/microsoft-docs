# Read only permission for shared mailboxes
## Introduction
Only 2 permissions are allowed for shared mailboxes: FullAccess and SendAs. This guide shows how to grant read only access for one user to a shared mailbox.

That is, the delegate will not be able to delete / edit / move the items within the shared mailbox.

## Procedure
The PowerShell cmdlets `Add-MailboxPermission` and `Add-RecipientPermission` can only be used to grand FullAccess and SendAs / SendOnBehalf permissions.

For read only access, we can only use the cmdlet `Add-MailboxFolderPermission` but this cmdlet is limited to an individual folder.

Thus, to achieve it, we need a script to iterate through each folder inside the mailbox and grant *Reviewer* access.

1. [Connect to Exchange Online PowerShell](https://docs.microsoft.com/en-us/powershell/exchange/connect-to-exchange-online-powershell).

2. Iterate through the shared mailbox folder and grant permission to the user.

```
$sharedMailbox = "shared@contoso.com"
$delegate = "kevin@consoto.com"

# Grant Reviewer access to the root folder
Add-MailboxFolderPermission -identity $sharedMailbox -AccessRights Reviewer -User $delegate

# Iterate through each folder and grant Reviewer access
$folders = Get-MailboxFolderStatistics -Identity $sharedMailbox | Select -ExpandProperty FolderPath
ForEach($folder in $folders){
    $path = $folder.replace("/","\")
    Add-MailboxFolderPermission -identity "$($sharedMailbox):$path" -AccessRights Reviewer -User $delegate
}
```

Note: while running this script, you will note that some folders cannot be shared. This is because those are system generated folders such as Purge, Holds, Versions, ...
Those are safe to ignore.

### Remove read only permission
Similarly, to remove the above granted permissions, you can run the same script but using `Remove-MailboxFolderPermission` instead of `Add-MailboxFolderPermission`.

```
$sharedMailbox = "shared@contoso.com"
$delegate = "kevin@consoto.com"

# Remove Reviewer access to the root folder
Remove-MailboxFolderPermission -identity $sharedMailbox -AccessRights Reviewer -User $delegate

# Iterate through each folder and grant Reviewer access
$folders = Get-MailboxFolderStatistics -Identity $sharedMailbox | Select -ExpandProperty FolderPath
ForEach($folder in $folders){
    $path = $folder.replace("/","\")
    Remove-MailboxFolderPermission -identity "$($sharedMailbox):$path" -AccessRights Reviewer -User $delegate
}
```

## More information
[Add-MailboxFolderPermission (Exchange PowerShell) | Microsoft Docs](https://docs.microsoft.com/en-us/powershell/module/exchange/add-recipientpermission?view=exchange-ps)
[Remove-MailboxFolderPermission (Exchange PowerShell) | Microsoft Docs](https://docs.microsoft.com/en-us/powershell/module/exchange/remove-recipientpermission?view=exchange-ps)

