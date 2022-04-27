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
The main point of the strict policy compared to the standard one is moving all suspicious items to quarantine.

| Property | Standard policy | Strict policy |
| ------ | ------------- | -------- |
| ApplyPhishActionToIntraOrg | FALSE |
| BulkSpamAction | Quarantine |
| BulkThreshold | 4 |
| DownloadLink | FALSE |
| EnableEndUserSpamNotifications | TRUE |
| EnableLanguageBlockList | FALSE |
| EnableRegionBlockList | FALSE |
| EndUserSpamNotificationCustomFromAddress |  |
| EndUserSpamNotificationCustomFromName |  |
| EndUserSpamNotificationCustomSubject |  |
| EndUserSpamNotificationFrequency | 3 |
| EndUserSpamNotificationLanguage | Default |
| EndUserSpamNotificationLimit | 0 |
| FalsePositiveAdditionalRecipients |  |
| HighConfidencePhishAction | Quarantine |
| HighConfidenceSpamAction | Quarantine |
| IncreaseScoreWithBizOrInfoUrls | Off |
| IncreaseScoreWithImageLinks | Off |
| IncreaseScoreWithNumericIps | Off |
| IncreaseScoreWithRedirectToOtherPort | Off |
| InlineSafetyTipsEnabled | TRUE |
| IsDefault | FALSE |
| LanguageBlockList |  |
| MarkAsSpamBulkMail | On |
| MarkAsSpamEmbedTagsInHtml | Off |
| MarkAsSpamEmptyMessages | Off |
| MarkAsSpamFormTagsInHtml | Off |
| MarkAsSpamFramesInHtml | Off |
| MarkAsSpamFromAddressAuthFail | Off |
| MarkAsSpamJavaScriptInHtml | Off |
| MarkAsSpamNdrBackscatter | Off |
| MarkAsSpamObjectTagsInHtml | Off |
| MarkAsSpamSensitiveWordList | Off |
| MarkAsSpamSpfRecordHardFail | Off |
| MarkAsSpamWebBugsInHtml | Off |
| ModifySubjectValue |  |
| PhishSpamAction | Quarantine |
| PhishZapEnabled | TRUE |
| QuarantineRetentionPeriod | 30 |
| RedirectToRecipients |  |
| RegionBlockList |  |
| SpamAction | Quarantine |
| SpamZapEnabled | TRUE |
| TestModeAction | None |
| TestModeBccToRecipients |  |
| ZapEnabled | TRUE |

## Anti-phishing policy
| Property | Standard policy | Strict policy |
| ------ | ------------- | -------- |
| AuthenticationFailAction | Quarantine |
| EnableAuthenticationSafetyTip | FALSE |
| EnableAuthenticationSoftPassSafetyTip | FALSE |
| EnableFirstContactSafetyTips | FALSE |
| EnableMailboxIntelligence | TRUE |
| EnableMailboxIntelligenceProtection | FALSE |
| EnableOrganizationDomainsProtection | FALSE |
| EnableSimilarDomainsSafetyTips | FALSE |
| EnableSimilarUsersSafetyTips | FALSE |
| EnableSpoofIntelligence | TRUE |
| EnableSuspiciousSafetyTip | FALSE |
| EnableTargetedDomainsProtection | FALSE |
| EnableTargetedUserProtection | FALSE |
| EnableUnauthenticatedSender | TRUE |
| EnableUnusualCharactersSafetyTips | FALSE |
| EnableViaTag | TRUE |
| ExcludedDomains |  |
| ExcludedSenders |  |
| ImpersonationProtectionState | Manual |
| IsDefault | FALSE |
| MailboxIntelligenceProtectionAction | NoAction |
| MailboxIntelligenceProtectionActionRecipients |  |
| MailboxIntelligenceQuarantineTag | DefaultFullAccessPolicy |
| Name | Strict Preset Security Policy1643092490277 |
| PhishThresholdLevel | 1 |
| RecommendedPolicyType | Strict |
| SpoofQuarantineTag | DefaultFullAccessPolicy |
| TargetedDomainActionRecipients |  |
| TargetedDomainProtectionAction | NoAction |
| TargetedDomainQuarantineTag | DefaultFullAccessPolicy |
| TargetedDomainsToProtect |  |
| TargetedUserActionRecipients |  |
| TargetedUserProtectionAction | NoAction |
| TargetedUserQuarantineTag | DefaultFullAccessPolicy |
| TargetedUsersToProtect |  |
| TreatSoftPassAsAuthenticated | FALSE |


## ATP safe attachments
| Property | Standard policy | Strict policy |
| ------ | ------------- | -------- |
| Action | DeleteMessage |
| AdminDisplayName |  |
| BypassInboundMessages | FALSE |
| BypassOutboundMessages | FALSE |
| CustomExternalSubject |  |
| CustomFromAddress |  |
| CustomFromName |  |
| CustomInternalSubject |  |
| CustomNotifications | FALSE |
| EnableExternalSenderAdminNotifications | FALSE |
| EnableExternalSenderNotifications | FALSE |
| EnableFileFilter | TRUE |
| EnableInternalSenderAdminNotifications | FALSE |
| EnableInternalSenderNotifications | FALSE |
| ExternalSenderAdminAddress |  |
| FileTypes[0] | ace |
| FileTypes[1] | ani |
| FileTypes[2] | app |
| FileTypes[3] | cab |
| FileTypes[4] | docm |
| FileTypes[5] | exe |
| FileTypes[6] | iso |
| FileTypes[7] | jar |
| FileTypes[8] | jnlp |
| FileTypes[9] | reg |
| FileTypes[10] | scr |
| FileTypes[11] | vbe |
| FileTypes[12] | vbs |
| InternalSenderAdminAddress |  |
| ZapEnabled | TRUE |

## Summary
You might have noticed that both preset security policies can be manually replicated from a custom policy. So if you find it more convenient to build a custom policy from the preset, you can set the corresponding values instead of starting from scratch.

This can be done from the PowerShell cmdlet `Set-HostedContentFilterPolicy` for instance.



## More information
[Preset security policies - Office 365 | Microsoft Docs](https://docs.microsoft.com/en-us/microsoft-365/security/office-365-security/preset-security-policies?view=o365-worldwide)
