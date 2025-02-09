---
title: PowerShell Scripts
icon: material/powershell
hide:
  - toc
---

# PowerShell Modules

### PSWriteColor

``` pwsh
if (-not (Get-Module PSWriteColor -ListAvailable)) {
    Install-Module PSWriteColor -Scope CurrentUser -Force
}
Import-Module PSWriteColor
```

### SharePoint

``` pwsh
if (-not (Get-Module Microsoft.Online.SharePoint.PowerShell -ListAvailable)) {
    Install-Module -Name Microsoft.Online.SharePoint.PowerShell -Scope CurrentUser -Force
}
Import-Module Microsoft.Online.SharePoint.PowerShell
```

### Teams

``` pwsh
if (-not (Get-Module MicrosoftTeams -ListAvailable)) {
    Install-Module -Name MicrosoftTeams -Scope CurrentUser -Force
}
Import-Module MicrosoftTeams
```

### Exchange

``` pwsh
if (-not (Get-Module ExchangeOnlineManagement -ListAvailable)) {
    Install-Module -Name ExchangeOnlineManagement -Scope CurrentUser -Force
}
Import-Module ExchangeOnlineManagement
```