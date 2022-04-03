# User last sign in
## Introduction
To track the last sign-in activity per user account, many approaches have been mentioned on how to do it in bulk using PowerShell, but which one is reliable? We'll discuss it in depth.

### Get-MailboxStatistics
To have a report of all users' last sign in, administrators tend to rely on the `LastLogonTime` attribute from the `Get-MailboxStatistics` PowerShell cmdlet. This approach has to major drawbacks:
1. It requires the user to have a mailbox.
2. It includes events when system services access the mailbox.

This method is described in depth by Tony Redmond ([link](https://petri.com/get-mailboxstatistics-cmdlet-wrong/)). We will not discuss it further here, but the other attributes `LastLoggedOnUserAccount`, `LastUserAccessTime`, or `LastInteractionTime` aren't neither accurate. They all can be influenced by system services and are limited to activities performed on the mailbox.

### Azure AD sign-in logs
To this date, only the sign-in logs stored in the Azure AD admin center better reflects the users' activity. With the help of the `Get-AzureADAuditSignInLogs` cmdlet, we can get the sign-in logs for a specific user.

The limitation of this approach is that the oldest last sign-in event depends on the Azure AD license that you have.

| Report | Azure AD Free | Azure AD Premium P1 | Azure AD Premium P2 |
| ------ | ------------- | ------------------- | ------------------- |
| Sign-ins | 7 days | 30 days | 30 days |
| Audit logs | 7 days | 30 days | 30 days |

**What does it mean?**

It means that if you don't have any Azure AD license, if a user hasn't signed in for the last 7 days, this approach will not return any sign-in activity for that user. 

If you have an Azure AD license, you can have up to 30 days of sign-in activity log and have it extended by archiving the sign-in logs (detailed tutorial [here](https://docs.microsoft.com/en-us/azure/active-directory/reports-monitoring/quickstart-azure-monitor-route-logs-to-storage-account)).

## Procedure
To export the last sign-in logs per user account in your organization.

1. [Connect to Azure AD Preview](https://docs.microsoft.com/en-us/powershell/azure/active-directory/install-adv2?view=azureadps-2.0).

2. Export the sign-in logs as a CSV file

```
$result = @()
Get-AzureADUser | Foreach {
    Start-Sleep 1
    Write-Output "Processing $($_.UserPrincipalName)"
    $logs = Get-AzureADAuditSignInLogs -Filter "startsWith(userPrincipalName,'$($_.UserPrincipalName)')" -Top 1
    if($logs.CreatedDateTime -eq $null) {
        $lastLogon = "No sign-in recorded"
    }
    else {
        $lastLogon = $logs.CreatedDateTime
    }
    $Result += New-Object PSObject -property @{
        Name = $logs.UserDisplayName
        UserPrincipalName = $_.UserPrincipalName
        LastLogonTime = $lastLogon
    }
}
$result | Export-CSV "O365-LastLogon-Report.csv"
```

To preview the results before exporting as a CSV.

`$result | Format-List *`

## More information
[Get-AzureADAuditSignInLogs (AzureADPreview) - Microsoft Docs](https://docs.microsoft.com/en-us/powershell/module/azuread/get-azureadauditsigninlogs?view=azureadps-2.0-preview)