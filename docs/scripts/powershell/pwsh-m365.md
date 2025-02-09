---
title: M365 PS Snippets
icon: material/microsoft-azure
---

# M365 Snippets

## Exchange

``` pwsh
Import-Module -Name ExchangeOnlineManagement
Connect-ExchangeOnline -UserPrincipalName name@example.com
```
[Documentation](https://learn.microsoft.com/en-us/powershell/exchange/connect-to-exchange-online-powershell?view=exchange-ps)

### Mailboxes

#### All Tenant Mailboxes

``` pwsh
Get-Mailbox -ResultSize Unlimited `
    | % { Get-MailboxStatistics $_.UserPrincipalName `
    | Select DisplayName, ItemCount, TotalItemSize, } `
    | Export-CSV -Path .\export.csv -NoTypeInformation
```
Additional fields: `MailboxTypeDetail`, `DeletedItemCount`, `TotalDeletedItemSize`

#### Specific User

``` pwsh
Get-EXOMailbox -Identity name@example.com `
    | Get-EXOMailboxStatistics
```

#### Mailbox Rules

``` pwsh
Get-InboxRule -Mailbox name@example.com
```

``` pwsh
Get-InboxRule -Mailbox name@example.com | Select * | `
    Export-CSV -Path .\mboxrules.csv -NoTypeInformation`
```

### Calendars

#### View Calendar Permissions

``` pwsh
Get-MailboxFolderPermission `
    -Identity room@example.com:\Calendar | Format-List
```

#### Show Meeting Subject and Remove Organizer

``` pwsh
Set-CalendarProcessing -Identity "room@example.com" `
    -DeleteSubject $False `
    -AddOrganizerToSubject $False
```

### Teams

#### Get Outbound Dialing Policy

``` pwsh
Get-CsUserPolicyAssignment -Identity "name@example.com"
```

#### Disable Oubound Dialing

``` pwsh
# Before
Get-CsUserPolicyAssignment -Identity "name@example.com"

Grant-CsDialoutPolicy -Identity "name@example.com" `
    -PolicyName "DialoutCPCDisabledPSTNInternational"

# After
Get-CsUserPolicyAssignment -Identity "name@example.com"
```
