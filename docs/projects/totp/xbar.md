---
title: TOTP for xbar (macOS)
icon: simple/apple
---
# TOTP for xbar

Built specifically for [`xbar`](https://xbarapp.com/) (macOS port of [`bitbar`](https://github.com/p-e-w/argos)) this tool has two flavors: **with** [YubiKey](https://www.yubico.com/) support, or **without** *(legacy)*. 

!!! info

    Any future updates for this tool will focus on the YubiKey version only because it allows for significantly better security by default.

## YubiKey TOTP

!!! blankout inline end ""

    [:simple-github: View on GitHub :material-arrow-top-right:](https://github.com/bryandodd/xbar/blob/main/yubikey-support/mfa-yubikey.1d.sh){ .button-62 .md-button--primary }

YubiKey 5 supports up to [64 OATH-TOTP credentials](https://support.yubico.com/hc/en-us/articles/360013790319-How-many-accounts-can-I-register-my-YubiKey-with#:~:text=up%20to%2064%20OATH%2DTOTP%20credentials) saved directly to your device. While the [Yubico Authenticator app](https://www.yubico.com/products/yubico-authenticator/) can be used to access these keys, I needed something faster and more readily accessible when jumping between apps and sites. This Xbar module makes those credentials available to you from the macOS menu bar. 

### Notes

![](https://raw.githubusercontent.com/bryandodd/xbar/main/images/yubikey-2.png){ align=right }

* Works with Nerd Fonts. I prefer [`JetBrains Mono Nerd Font Propo`](https://www.nerdfonts.com/), but this can easily be changed to your liking. See the `FONT` variable.
* If your Yubikey is not inserted when the `xbar` application loads, or if your key is not detected for any other reason, you may see a "Yubikey not detected" message. Insert your Yubikey and click the "reload" option to refresh the extension and read OATH codes from your device. 
* If you have multiple Yubikeys and did not specify `ykUseSerial` and `ykDevice`, you will see a "Multiple Yubikeys detected" message. Update the variables and click the "reload" option to refresh the extension and read from the specified device only. 

![](https://raw.githubusercontent.com/bryandodd/xbar/main/images/yubikey-1.png)
/// caption
<sup>Yubikey not detected</sup>
///
* Tested and working with Yubikey OATH codes regardless of "touch" setting. If your code has been configured to require touch, no additional prompts will display on-screen, but your Yubikey will begin to flash after clicking the name of the code you with you retrieve. Tap your key as usual and the value will be recorded to the macOS clipboard.
* The script will determine if it is being run on Apple Silicon and set the `PATH` accordingly. 

### Dependencies

* [yubikey-manager](https://github.com/Yubico/yubikey-manager) - `brew install ykman`

### Settings

=== "Operational Settings"

    | Setting | Default | Note |
    |---------|---------|------|
    | `ykPassRequired` | `false` | Change to `true` if your Yubikey has been configured to require a password for OATH use. When `true`, `ykPassword` must also be supplied for proper operation. |
    | `ykPassword` | `YUBIKEY-OATH-PASSWORD` | Required when `ykPassRequired` is `true`. This value is a plain-text string of your Yubikey OATH password. |
    | `ykOrderOverride` | `false` | OATH codes are read from Yubikey and displayed in alphabetical order. To override this behavior, or to selectively group or omit codes, set value to `true`. When set to `true`, you must provide *(at minimum)* the values for all three `groupPrimary` settings. |
    | `ykUseSerial` | `false` | Change to `true` to directly target a specific Yubikey if you have (or expect to have) multiple Yubikeys connected to your machine at one time. When `true`, `ykDevice` must also be supplied for proper operation. |
    | `ykDevice` | `99999999` | Required when `ykUseSerial` is `true`. The Serial Number of the Yubikey you want the module to read from. |
    | `iconEnable` | `true` | Designed to be used in conjunction with [Nerd Fonts](https://www.nerdfonts.com/). If set to `true`, values specified in the `iconArray` will be used to "look up" which Nerd Font glyphs should be used with each OATH code from the Yubikey. |
    | `groupSecondaryEnable` | `true` | Enable support for grouping Yubikey OATH keys in two groups. |
    | `submenuEnable` | `false` | Enable nesting of OATH keys under their group name. Causes only one group of keys to be visible at a time. |

=== "Configuration Settings"

    | Setting | Note |
    |---------|------|
    | `groupPrimaryName` / `groupSecondaryName` | Heading name for groups of OATH codes. |
    | `groupPrimaryColor` / `groupSecondaryColor` | Heading colors for groups of OATH codes. When `submenuEnable` is true, headings become interactive and the colors become darker. Using the IF statement, manually specify a lighter color for submenus to keep colors consistent between modes.<br />`$(if [ "$submenuEnable" = "true" ]; then echo "#807FFE"; else echo "BLUE"; fi;)` |
    | `groupPrimaryList` / `groupSecondaryList` | Array of OATH keys. The format must match the output from Yubikey. To see which keys are available on your device, run `ykman oath accounts list`. Values must be contained within double-quotes and do not separate with commas (`,`). |
    | `iconArray` | Used only if `iconEnable` is `true`. Array of labels paired with Nerd Font glyphs. Separate key/value with a colon (`:`), use double quotes (`"`), and do not separate value pairs with commas (`,`). If an OATH key is found that does not have a matching glyph, a circle and exclaimation glyph will display for that OATH entry. |


---

## YubiKey Terminal ![Supplemental](https://img.shields.io/badge/Supplemental-7932a8?style=flat&logo=htmx&logoColor=white)

![](https://raw.githubusercontent.com/bryandodd/xbar/main/images/yubikey-terminal.png){ width="450", align=right }

This supplemental tool has four (4) modes of operation. It can be executed as a stand-alone script, or integrated into your shell profile. 

[:simple-github: View on GitHub :material-arrow-top-right:](https://github.com/bryandodd/xbar/blob/main/terminal-yubikey/yubikey-mfa.sh){ .button-62 .md-button--primary }

1. No parameter passed.
    * OATH keys will be read from Yubikey and you will be prompted to select the number of the key you want to capture. Code will be returned and its value copied to the macOS clipboard. 
2. Pass "list" as the only parameter.
    * OATH keys will be read from Yubikey. They will be numbered, but you will not be prompted to select a number, and no codes will be returned or copied to the clipboard.
3. Pass the number identifier for the key you're wanting.
    * The OATH code for the specified key will be returned and copied to macOS clipboard. 
4. Pass a string containing the name of the OATH key.
    * If you provide a partial string, a match will be attempted but could fail if there are multiple possible selections each containing the string you provided. Potential matches will be returned allowing you to make a more specific request.
    * You can also choose to provide the entire Yubikey-Manager compliant string identifier: `<identity>:<account>`. This will always ensure a positive match if the key exists within the Yubikey you have inserted. 
    * Additional data about the OATH code is omitted from output - this means that only the OATH code itself is emitted and is therefore more suitable for use in programmatic applications.

### Notes

* If your Yubikey is not inserted when the script is executed, or if your key is not detected for any other reason, you may see the "Yubikey not detected" message.
* If you have multiple Yubikeys and did not specify `ykUseSerial` and `ykDevice`, you will see a "Multiple Yubikeys detected" message. 
* Tested and working with Yubikey OATH codes regardless of "touch" setting. If your code has been configured to require touch, prompts will appear on-screen as required and your Yubikey will begin to flash to indicate contact is required. Tap your key as usual and the value will be displayed on-screen and recorded to the macOS clipboard.
* If you have multiple keys loaded with very similar names, you may have to use the full OATH key name when attempting to retrieve codes by string rather than number identifier. Consider the following examples:

### Dependencies

* [yubikey-manager](https://github.com/Yubico/yubikey-manager) - `brew install ykman`

### Settings

=== "Operational Settings"

    | Setting | Default | Note |
    |---------|---------|------|
    | `ykPasswordRequired` | `false` | Change to `true` if your Yubikey has been configured to require a password for OATH use. When `true`, `ykPasswordString` must also be supplied for proper operation. |
    | `ykPasswordString` | `some-password` | Required when `ykPasswordRequired` is `true`. This value is a plain-text string of your Yubikey OATH password. |
    | `ykUseSerial` | `false` | Change to `true` to directly target a specific Yubikey if you have (or expect to have) multiple Yubikeys connected to your machine at one time. When `true`, `ykDevice` must also be supplied for proper operation. |
    | `ykDevice` | `99999999` | Required when `ykUseSerial` is `true`. The Serial Number of the Yubikey you want the module to read from. |
    | `ykMaxKeys` | `64` | Yubikey devices with firmware 5.7.x now support 64 OATH codes stored on-device (versus 32 previously). |

=== "Configuration Settings"

    *No additional settings available.*

#### Specifying similarly named MFA codes

``` title="Example 1"
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

``` title="Example 2"
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

---

!!! warning "The legacy version is deprecated because it isn't secure."

    The original iterations of my `xbar` module remain purely for historical purposes. Anyone is welcome to experiment and play with the scripts below, but please do not use these for real-world security.

## Legacy TOTP ![Deprecated](https://img.shields.io/badge/Deprecated-3f51b5?style=flat&logo=htmx&logoColor=white)

!!! blankout inline end ""

    [:simple-github: View on GitHub :material-arrow-top-right:](https://github.com/bryandodd/xbar/blob/main/legacy/mfa-codes.1d.sh){ .button-62 .md-button--primary }

This use case is deprecated and should considered insecure. It relies on the TOTP seeds being saved to your filesystem and accessible to the module itself. 

### Notes

![](https://raw.githubusercontent.com/bryandodd/xbar/main/images/mfa-codes-1.png){ align=right }

*  Allows for two modes of use:
    * Traditionally identified within the menu bar as an application name, or;
    * Specify one specific account to be premanently visible on the menu bar, it's TOTP code visible and refreshed at all times.
*  Tested and works well with Nerd Fonts. My preference is `JetBrains Mono Nerd Font` but this can easily be changed to your liking.
*  If you wish to ENABLE the default code, consider changing the script name to execute every five (5) or fewer seconds. By default, the script will not display a default code and therefore refreshes only once per day.

![](https://raw.githubusercontent.com/bryandodd/xbar/main/images/mfa-codes-2.png) 
/// caption
<sup>Default code enabled</sup>
///

### Dependencies

* [OATH-Toolkit](https://www.nongnu.org/oath-toolkit/) - `brew install oath-toolkit`

### Settings

=== "Operational Settings"

    | Setting | Default | Note |
    |---------|---------|------|
    | `ENABLE_DEFAULT_CODE` | `false` | Causes the "default" code to be displayed in the menu bar. |
    | `ENABLE_DEFAULT_COPY` | `true` | Adds a "Copy Default Code" option to the top of the menu. If set to false, only the code is displayed in the menu. |
    | `ONLY_ONE_GROUP` | `false` | Causes all groups other than "group1" to be ignored. Heading name for Group 1 will not be displayed. |
    | `ENABLE_SUBMENU` | `false` | Causes codes to be rolled-up under their group heading rather than displayed together in the top level of the menu. |

=== "Configuration Settings"

    | Setting | Note |
    |---------|------|
    | `DEFAULT_NAME` | Relevant when `ENABLE_DEFAULT_CODE` is true. Displays a TOTP code directly in the top menu bar. |
    | `DEFAULT_KEY` | Relevant when `ENABLE_DEFAULT_CODE` is true. Displays a TOTP code directly in the top menu bar. |
    | `group1Name` / `group2Name` | Heading name for the group of codes. |
    | `group1Color` / `group2Color` | Heading color for the group name. |
    | `mfaGroup1` / `mfaGroup2` | Array of TOTP keys. Format is `<name>:<key>`. Values must be contained within double-quotes and do not separate with commas (`,`). |

---

## Legacy Terminal ![Deprecated](https://img.shields.io/badge/Deprecated-3f51b5?style=flat&logo=htmx&logoColor=white)

![](https://raw.githubusercontent.com/bryandodd/xbar/main/images/mfa-terminal.png){ align=right }

This was the first tool I created to assist in management and use of MFA codes, predating my discovery of `xbar`. Use requires two files:

1. a file with MFA key strings and metadata separated by `tab` characters
2. bash script to cherry-pick individual keys and decode the TOTP token upon request

[:simple-github: View on GitHub :material-arrow-top-right:](https://github.com/bryandodd/xbar/blob/main/terminal-legacy/legacy-mfa.sh){ .button-62 .md-button--primary }

[OATH-Toolkit](https://www.nongnu.org/oath-toolkit/) is required. Install with `brew install oath-toolkit`.

The MFA key file should be saved to your local system with a name similar to `.mfa-codes`. For the sake of example we'll say this is saved at `~/.mfa/.mfa-codes`. This file should contain information structured as follows: 
```
short-name {tab} mfa-code {tab} username (or email) {tab} long-description, {newline}
```

The contents of the file should look something like this:
```title="~/.mfa/.mfa-codes"
# TOKENS

aws-prod	ABC123ABC123ABC1	user@email.com	AWS (Prod)
aws-dev	ABC123ABC123ABC1	user2@email2.com	AWS (Dev)
microsoft	ABC123ABC123ABC1	user3@email3.com	O365
vpn	ABC123ABC123ABC1	user4	Office VPN
lastpass	ABC123ABC123ABC1	user5@email5.com	LastPass
gitlab	ABC123ABC123ABC1	user6@email6.com	GitLab (Personal)
outlook	ABC123ABC123ABC1	user7@outlook.com	Outlook (Personal)
paypal	ABC123ABC123ABC1	user8@email8.com	PayPal (Personal)
```

Usage is very basic. Add the script to your system path so that it's accessible from anywhere as simply `mfa`, then
* pass `list` to retrieve a parsed listing of everything from the `.mfa-codes` file, and;
* pass the short-name to get the TOTP code for that entry.