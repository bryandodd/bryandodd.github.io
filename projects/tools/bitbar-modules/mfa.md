---
layout: page
title: MFA for Gnome
parent: tools
grand_parent: projects
nav_order: 2
permalink: /projects/tools/bitbar/mfa
---

# MFA for Gnome
{: .no_toc }

Built specifically for [`bitbar`](https://github.com/p-e-w/argos), this tool reads OATH codes from [YubiKey](https://www.yubico.com/), displays them on-screen and copies them to the system clipboard. 

[jump to parent repo](https://github.com/bryandodd/bitbar){: .btn }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

---

## YubiKey MFA Support

Current
{: .label .label-green }

![](https://raw.githubusercontent.com/bryandodd/bitbar/main/images/bitbar-mfa-yk1.png)  ![](https://raw.githubusercontent.com/bryandodd/bitbar/main/images/bitbar-mfa-yk2.png) 

### Notes
{: .no_toc }

* Tested with Nerd Fonts. My preference for Gnome is `Overpass Nerd Font`, but this can easily be changed to your liking. See the `FONT` and `LABELFONT` variables.
* If your Yubikey is not inserted when the `bitbar` application loads, or if your key is not detected for any other reason, you may see a "Yubikey not detected" message. Insert your Yubikey and click the "reload" option to refresh the extension and read OATH codes from your device.
* Tested and working with Yubikey OATH codes regardless of "touch" setting. If your code has been configured to require touch, no additional prompts will display on-screen, but your Yubikey will begin to flash after clicking the name of the code you with you retrieve. Tap your key as usual and the value will be recorded to the system clipboard.

### Dependencies

* [yubikey-manager](https://github.com/Yubico/yubikey-manager)

### Operational Settings

| Setting | Default | Note |
|---------|---------|------|
| `ykPassRequired` | `false` | Change to `true` if your Yubikey has been configured to require a password for OATH use. When `true`, `ykPassword` must also be supplied for proper operation. |
| `ykPassword` | `YUBIKEY-OATH-PASSWORD` | Required when `ykPassRequired` is `true`. This value is a plain-text string of your Yubikey OATH password. |
| `ykOrderOverride` | `false` | OATH codes are read from Yubikey and displayed in alphabetical order. To override this behavior, or to selectively group or omit codes, set value to `true`. When set to `true`, you must provide at minimum the values for all three `groupPrimary` settings (identified below). |
| `iconEnable` | `true` | Designed to be used in conjunction with [Nerd Fonts](https://www.nerdfonts.com/). If set to `true`, values specified in the `iconArray` will be used to "look up" which Nerd Font glyphs should be used with each OATH code from the Yubikey. |
| `groupSecondaryEnable` | `true` | Enable support for grouping Yubikey OATH keys in two groups. |

### Configuration Settings

| Setting | Note |
|---------|------|
| `ykOathList` | Intentionally empty. Do not modify. |
| `groupPrimaryName` / `groupSecondaryName` | Heading name for groups of OATH codes. |
| `groupPrimaryColor` / `groupSecondaryColor` | Heading colors for groups of OATH codes. |
| `groupPrimaryList` / `groupSecondaryList` | Array of OATH keys. The format must match the output from Yubikey. To see which keys are available on your device, run `ykman oath accounts list`. Values must be contained within double-quotes and do not separate with commas (`,`). |
| `iconArray` | Used only if `iconEnable` is `true`. Array of labels paired with Nerd Font glyphs. Separate key/value with a colon (`:`), use double quotes (`"`), and do not separate value pairs with commas (`,`). If an OATH key is found that does not have a matching glyph, a circle and exclaimation glyph will display for that OATH entry. |

---

## YubiKey Terminal Support

Supplemental
{: .label .label-blue }

![](https://raw.githubusercontent.com/bryandodd/bitbar/main/images/term-yk1.png)

This supplemental tool has four (4) modes of operation. It can be executed as a stand-alone script, or integrated into your shell profile. 
1. No parameter passed.
    * OATH keys will be read from Yubikey and you will be prompted to select the number of the key you want to capture. Code will be returned and its value copied to the system clipboard. 
2. Pass "list" as the only parameter.
    * OATH keys will be read from Yubikey. They will be numbered, but you will not be prompted to select a number, and no codes will be returned or copied to the clipboard.
3. Pass the number identifier for the key you're wanting.
    * The OATH code for the specified key will be returned and copied to system clipboard. 
4. Pass a string containing the name of the OATH key you're wanting.
    * If you provide a partial string, a match will be attempted but could fail if there are multiple possible selections each containing the string you provided. Potential matches will be returned allowing you to make a more specific request.
    * You can also choose to provide the entire Yubikey-Manager compliant string identifier: `<identity>:<account>`. This will always ensure a positive match if the key exists within the Yubikey you have inserted. 
    * Additional data about the OATH code is omitted from output - this means that only the OATH code itself is emitted and is therefore more suitable for use in programmatic applications.

> As written, this script is designed to be copied into your bash or zsh profile. Note the `mfa` alias defined on line 25.

### Notes
{: .no_toc }

* If your Yubikey is not inserted when the script is executed, or if your key is not detected for any other reason, you may see the message "Yubikey not detected." 
* If your OS distribution includes default aliases for the individual number characters (1 through 9, aliased to `cd -N`), the `unalias` commands may be necessary for the script to work properly by passing a number in as a parameter.
    * If you have aliases for the number characters that you must not change, prefix single digits with zero (i.e., instead of `5`, pass `05`).
* Tested and working with Yubikey OATH codes regardless of "touch" setting. If your code has been configured to require touch, prompts will appear on-screen as required and your Yubikey will begin to flash to indicate contact is required. Tap your key as usual and the value will be displayed on-screen and recorded to the system clipboard.
* If you have multiple keys loaded with very similar names, you may have to use the full OATH key name when attempting to retrieve codes by string rather than number identifier.

### Dependencies

* [yubikey-manager](https://github.com/Yubico/yubikey-manager)

### Operational Settings

| Setting | Default | Note |
|---------|---------|------|
| `ykPasswordRequired` | `false` | Change to `true` if your Yubikey has been configured to require a password for OATH use. When `true`, `ykPasswordString` must also be supplied for proper operation. |
| `ykPasswordString` | `some-password` | Required when `ykPasswordRequired` is `true`. This value is a plain-text string of your Yubikey OATH password. |

### Configuration Settings
{: .no_toc }

No additional settings available.

### Specifying similarly named MFA codes
{: .no_toc }

```
You have 3 OATH codes saved to your Yubikey with the following Issuer and Accounts:
* AWS [Prod]:usera@workemail.com
* AWS [Dev Root]:usera
* AWS [Personal]:usera@homeemail.com

You will have difficulty attempting to capture codes if all you are passing to the script is "AWS":

$ ./yubikey-mfa.sh "AWS"
Error: Multiple matches, please make the query more specific.

AWS [Prod]:usera@workemail.com
AWS [Dev Root]:usera
AWS [Personal]:usera@homeemail.com

This can be resolved by passing the full Issuer string:
$ ./yubikey-mfa.sh "AWS [Dev Root]"
```

```
You have 3 OATH codes saved to your Yubikey with the following Issuer and Accounts:
* AWS:usera@workemail.com
* AWS:usera
* AWS:usera@homeemail.com

$ ./yubikey-mfa.sh "AWS"
Error: Multiple matches, please make the query more specific.

AWS:usera@workemail.com
AWS:usera
AWS:usera@homeemail.com

In this scenario, the only resolution is to use the full OATH name as known to Yubikey:
$ ./yubikey-mfa.sh "AWS:usera@homeemail.com"
```
