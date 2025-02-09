---
title: AWS PS Scripts
icon: simple/amazoniam
---

# AWS PowerShell Scripts

## AWS-Connector

Ensure PowerShell modules are saved to the correct directory. Example: `%UserProfile%\Documents\PowerShell\Modules\Custom\AWS-Connector.psm1`

- Modify line 12 to include the correct MFA ARN for your AWS IAM user.
- Modify line 24 and 42 to replace `AWS_ACCOUNT_ID` and/or `REGION` with the appropriate values for your ECR registry.

Assumes you've already properly configured the CLI and set your credentials. <br />*This module installs `PSWriteColor` if not previously installed.*

``` pwsh linenums="1" hl_lines="12 24 42" title="AWS-Connector.psm1"
if (-not (Get-Module PSWriteColor -ListAvailable)) {
    Install-Module PSWriteColor -Scope CurrentUser -Force
}
Import-Module PSWriteColor

# Connect to AWS using MFA. The connection will be valid for the current PowerShell session only.
function Connect-AWS {
    Write-Output "== AWS MFA CLI Login ==`r`n"
    do {
        $MFAcode = Read-Host "MFA Code "
    } while ($MFAcode.Length -ne 6)
    $MFAarn = "arn:aws:iam::123456789876:mfa/name@example.com"
    $sessionInfo = aws sts get-session-token --serial-number $($MFAarn) --token-code $($MFAcode) | ConvertFrom-Json

    Write-Color -T "Warning:`r`nThis login is only valid in the current session!`r`n" -C Yellow -ShowTime

    Write-Color -T "Access Key: ","$($sessionInfo.Credentials.AccessKeyId)" -C White,Green
    Write-Color -T "Secret Key: ","$($sessionInfo.Credentials.SecretAccessKey)" -C White,Green
    Write-Color -T "Session Token: ","$($sessionInfo.Credentials.SessionToken)`r`n" -C White,Green

    $Env:AWS_ACCESS_KEY_ID="$($sessionInfo.Credentials.AccessKeyId)"
    $Env:AWS_SECRET_ACCESS_KEY="$($sessionInfo.Credentials.SecretAccessKey)"
    $Env:AWS_SESSION_TOKEN="$($sessionInfo.Credentials.SessionToken)"
    $Env:AWS_DEFAULT_REGION="XX-XXXX-X"

    aws sts get-caller-identity
}

# Identify which AWS account and IAM user you are currently authenticated as.
function Get-AWSIdent {
    aws sts get-caller-identity
    if (($null -eq $Env:AWS_ACCESS_KEY_ID) -or ($null -eq $Env:AWS_SECRET_ACCESS_KEY) -or ($null -eq $Env:AWS_SESSION_TOKEN)) {
        Write-Color -T "Environment variables are not set!" -C Red
        Write-Color -T "Use ","'Connect-AWS' ","to authenticate." -C White,Blue,White
        break;
    }
}

# Connect to AWS ECR to enable "docker push" commands. You must use "Connect-AWS" first.
function Register-ECR {
    Write-Output "== AWS ECR Authentication ==`r`n"
    aws ecr get-login-password --region XX-XXXX-X | docker login --username AWS --password-stdin AWS_ACCOUNT_ID.dkr.ecr.REGION.amazonaws.com
}
```

### Functions

`Connect-AWS`

:   Prompts for your 6-digit TOTP value and uses the AWS CLI to authenticate with MFA. The temporary `Access Key ID`, `Secret Access Key`, and `Session Token` are displayed and set as local environment variables, enabling AWS CLI command execution without having to pass a profile.

`Get-AWSIdent`

:   Quikly calls `#!bash aws sts get-caller-identity` to report which credentials you've authenticated with, or reports that you aren't authenticated.

`Register-ECR`

:   Enables use of AWS ECR via Docker to allow `#!bash docker push` commands.