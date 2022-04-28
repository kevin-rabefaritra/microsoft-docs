# [Draft] Preset security policies in EOP
## Introduction
Preset security policies are by default recommended for applying different levels of pre-defined policies in EOP.
The main advantage is to avoid creating a custom policy for different layers of EOP:
- Inbound anti-spam policy
- Anti-phishing policy
- ATP safe links policy
- ATP safe attachments policy

At the time we are writing this review, there are two preset security policies: the standard protection and the strict protection.

Once the standard or strict preset policy is enabled on the tenant, any default or built-in policy is overridden.

> [...] the settings of the Strict protection policy override the settings of the Standard protection policy, which overrides the settings from a custom policy, which overrides the settings from the Built-in protection preset security policy [...].

In this review, we will quickly go through the standard and the strict security policy and compare the configuration that they use.

## Anti-spam policy

| Property | Standard policy | Strict policy |
| ------ | ------------- | -------- |
| QuarantineRetentionPeriod | 30 | 30| 
| EndUserSpamNotificationFrequency | 3 | 3| 
| TestModeAction | None | None| 
| IncreaseScoreWithImageLinks | Off | Off| 
| IncreaseScoreWithNumericIps | Off | Off| 
| IncreaseScoreWithRedirectToOtherPort | Off | Off| 
| IncreaseScoreWithBizOrInfoUrls | Off | Off| 
| MarkAsSpamEmptyMessages | Off | Off| 
| MarkAsSpamJavaScriptInHtml | Off | Off| 
| MarkAsSpamFramesInHtml | Off | Off| 
| MarkAsSpamObjectTagsInHtml | Off | Off| 
| MarkAsSpamEmbedTagsInHtml | Off | Off| 
| MarkAsSpamFormTagsInHtml | Off | Off| 
| MarkAsSpamWebBugsInHtml | Off | Off| 
| MarkAsSpamSensitiveWordList | Off | Off| 
| MarkAsSpamSpfRecordHardFail | Off | Off| 
| MarkAsSpamFromAddressAuthFail | Off | Off| 
| MarkAsSpamBulkMail | On | On| 
| MarkAsSpamNdrBackscatter | Off | Off| 
| LanguageBlockList |  | | 
| RegionBlockList |  | | 
| **HighConfidenceSpamAction** | Quarantine | Quarantine| 
| **SpamAction** | MoveToJmf | Quarantine| 
| EnableEndUserSpamNotifications | True | True| 
| DownloadLink | False | False| 
| EnableRegionBlockList | False | False| 
| EnableLanguageBlockList | False | False| 
| EndUserSpamNotificationCustomFromAddress |  | | 
| EndUserSpamNotificationCustomFromName |  | | 
| EndUserSpamNotificationCustomSubject |  | | 
| EndUserSpamNotificationLanguage | Default | Default| 
| EndUserSpamNotificationLimit | 0 | 0| 
| BulkThreshold | 6 | 4| 
| AllowedSenders |  | | 
| AllowedSenderDomains |  | | 
| BlockedSenders |  | | 
| BlockedSenderDomains |  | | 
| ZapEnabled | True | True| 
| InlineSafetyTipsEnabled | True | True| 
| **BulkSpamAction** | MoveToJmf | Quarantine| 
| **PhishSpamAction** | Quarantine | Quarantine| 
| SpamZapEnabled | True | True| 
| PhishZapEnabled | True | True| 
| ApplyPhishActionToIntraOrg | False | False| 
| **HighConfidencePhishAction** | Quarantine | Quarantine| 
| SpamQuarantineTag | DefaultFullAccessPolicy | DefaultFullAccessPolicy| 
| HighConfidenceSpamQuarantineTag | DefaultFullAccessPolicy | DefaultFullAccessPolicy| 
| PhishQuarantineTag | DefaultFullAccessPolicy | DefaultFullAccessPolicy| 
| HighConfidencePhishQuarantineTag | AdminOnlyAccessPolicy | AdminOnlyAccessPolicy| 
| BulkQuarantineTag | DefaultFullAccessPolicy | DefaultFullAccessPolicy| 

## Anti-phishing policy

| Property | Standard policy | Strict policy |
| ------ | ------------- | -------- |
|ImpersonationProtectionState | Automatic | Automatic| 
|EnableTargetedUserProtection | True | True| 
|EnableMailboxIntelligenceProtection | True | True| 
|EnableTargetedDomainsProtection | True | True| 
|EnableOrganizationDomainsProtection | True | True| 
|EnableMailboxIntelligence | True | True| 
|EnableFirstContactSafetyTips | False | False| 
|EnableSimilarUsersSafetyTips | True | True| 
|EnableSimilarDomainsSafetyTips | True | True| 
|EnableUnusualCharactersSafetyTips | True | True| 
|**TargetedUserProtectionAction** | Quarantine | Quarantine| 
|TargetedUserQuarantineTag | DefaultFullAccessPolicy | DefaultFullAccessPolicy| 
|**MailboxIntelligenceProtectionAction** | MoveToJmf | Quarantine| 
|MailboxIntelligenceQuarantineTag | DefaultFullAccessPolicy | DefaultFullAccessPolicy| 
|**TargetedDomainProtectionAction** | Quarantine | Quarantine| 
|TargetedDomainQuarantineTag | DefaultFullAccessPolicy | DefaultFullAccessPolicy| 
|**AuthenticationFailAction** | MoveToJmf | Quarantine| 
|SpoofQuarantineTag | DefaultFullAccessPolicy | DefaultFullAccessPolicy| 
|EnableSpoofIntelligence | True | True| 
|EnableViaTag | True | True| 
|EnableUnauthenticatedSender | True | True| 
|EnableSuspiciousSafetyTip | False | True| 
|**PhishThresholdLevel** | 2 | 3| 

## Anti-malware
For the anti-malware layer, the standard and strict security policies are the same.

| Property | Standard policy | Strict policy |
| ------ | ------------- | -------- |
| BypassInboundMessages | False | False|
| BypassOutboundMessages | False | False|
| Action | DeleteMessage | DeleteMessage|
| FileTypeAction | Quarantine | Quarantine|
| IsDefault | False | False|
| CustomNotifications | False | False|
| EnableInternalSenderNotifications | False | False|
| EnableExternalSenderNotifications | False | False|
| EnableInternalSenderAdminNotifications | False | False|
| EnableExternalSenderAdminNotifications | False | False|
| EnableFileFilter | True | True|
| FileTypes | ace ani app cab docm exe iso jar jnlp reg scr vbe vbs | ace ani app cab docm exe iso jar jnlp reg scr vbe vbs|
| QuarantineTag | AdminOnlyAccessPolicy | AdminOnlyAccessPolicy|
| ZapEnabled | True | True|

## Summary
Both preset security policies can be manually replicated from a custom policy. If you want to build a custom policy from scratch, you can set the corresponding values instead of enabling the preset security policies.

This can be done from the PowerShell cmdlet `Set-HostedContentFilterPolicy` for instance.

### 

## More information
[Preset security policies - Office 365 | Microsoft Docs](https://docs.microsoft.com/en-us/microsoft-365/security/office-365-security/preset-security-policies?view=o365-worldwide)
