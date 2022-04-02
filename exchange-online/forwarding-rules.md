# Mailbox forwarding
## Introduction
Despite having the ability to restrict internal users from automatically forwarding emails outside the organization via the anti-spam outbound policy, some administrators might want to know whose mailboxes are forwarding emails externally.

There are two ways for users to setup automatic forwarding:
1. By creating an inbox rule.
2. By setting up automatic forwarding from their Outlook client.

This PowerShell script will give you a report that includes every inbox rule / forwarding configuration that automatically forwards emails.

## Procedure
1. [Connect to Exchange Online PowerShell](https://docs.microsoft.com/en-us/powershell/exchange/connect-to-exchange-online-powershell?view=exchange-ps).

2. To export the inbox rules / forwarding rules for every mailbox in the organization.

```
# Initiate the rules as array
$rules = @()

# Process each mailbox (including shared mailboxes)
Get-Mailbox -ResultSize Unlimited | ForEach {
    Start-Sleep 1
    $smtpAddress = $_.PrimarySmtpAddress
    Write-Output "Processing $($smtpAddress)..."

    # Check for inbox rules
    Get-InboxRule -Mailbox $_.PrimarySmtpAddress | Where ForwardTo -Like "*SMTP*" | Foreach {
        $rule = New-Object -TypeName psobject
        $rule | Add-Member -MemberType NoteProperty -Name "User" -Value $smtpAddress
        $rule | Add-Member -MemberType NoteProperty -Name "Forward to" -Value $_.ForwardTo
        $rule | Add-Member -MemberType NoteProperty -Name "Rule" -Value $_.Description
        $rules += $rule
    }

    # Check for automatic forwarding rules
    if($_.DeliverToMailboxAndForward -and $_.ForwardingSmtpAddress) {
        $rule = New-Object -TypeName psobject
        $rule | Add-Member -MemberType NoteProperty -Name "User" -Value $smtpAddress
        $rule | Add-Member -MemberType NoteProperty -Name "Forward to" -Value $_.ForwardingSmtpAddress
        $rule | Add-Member -MemberType NoteProperty -Name "Rule" -Value "Auto-forward configuration"
        $rules += $rule
    }
}

# Export the output as a CSV file
$rules | Export-Csv -Path "inbox_rules.csv"
```

To check the results before exporting to a CSV file, use:

`$rules | Format-List *`

## More information
[Get-InboxRule (Exchange PowerShell) | Microsoft Docs](https://docs.microsoft.com/en-us/powershell/module/exchange/get-inboxrule?view=exchange-ps)