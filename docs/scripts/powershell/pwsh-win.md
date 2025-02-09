---
title: Windows PS Snippets
icon: material/microsoft-windows
---

# Windows Snippets

## System Events

[Documentation](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.diagnostics/get-winevent?view=powershell-7.4)

### Search Logs

``` pwsh
Get-WinEvent -FilterHashtable @{'LogName' = 'System'; 'ProviderName' = 'Microsoft-Windows-Kernel-General'; 'Id' = 1}
```

``` pwsh
$AuditEvents = Get-WinEvent -FilterHashtable @{'LogName' = 'Security'; 'ProviderName' = 'Microsoft-Windows-Security-Auditing'; 'Id' = 4616} | Select *
```

``` pwsh
$TimeEvents=Get-WinEvent -FilterHashtable @{'Path' = 'C:\Users\Administrator\Desktop\saved-file.evtx'; 'LogName' = 'System'; 'ProviderName' = 'Microsoft-Windows-Kernel-General'; 'Id' = 1} | Select *
```