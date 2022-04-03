# Distribution lists
## Introduction
A common monitoring operation for Exchange administrators is to have a report of the existing distribution lists and their members.

Note: for shared mailboxes, please refer to my other article.

## Procedure
To export the existing distribution lists and their members as a CSV file.

1. [Connect to Exchange Online PowerShell](https://docs.microsoft.com/en-us/powershell/exchange/connect-to-exchange-online-powershell).

2. Fetch the distribution lists and export the result as a CSV.

```
$result = Get-DistributionGroup -ResultSize Unlimited
$result | ForEach-Object {
    $groupObject = Get-DistributionGroup -Identity $_.PrimarySmtpAddress
    $group = $groupObject.DisplayName
    $members = ''
    $memberAddresses = ''
    $PrimarySmtpAddress = $groupObject.PrimarySmtpAddress
    $memberPermissions = ''
    Foreach($member in Get-DistributionGroupMember $_.PrimarySmtpAddress -resultsize unlimited) {
        $members = $members + $member.Name + ";"
        $memberAddresses = $memberAddresses + $member.WindowsLiveID + ";"
    }
    New-Object -TypeName PSObject -Property @{
        GroupName = $group
        Members = $members
        MemberAddresses = $memberAddresses
        PrimarySmtpAddress = $PrimarySmtpAddress
    }
} | Export-CSV "Distribution-Group-Members.csv" -NoTypeInformation -Encoding UTF8
```