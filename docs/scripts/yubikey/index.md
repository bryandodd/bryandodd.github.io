---
title: Yubikey Prerequisites
icon: simple/yubico
---

# Yubikey Setup & Configuration

!!! info "Annotations"

     Annotations are used throughout some of the examples below. Clicking the <span style="color: yellow">:octicons-feed-plus-16:{ title="annotation icon" }</span> symbol reveals additional comments intended for clarity.

## Tools and Requirements

There are a few prerequisites you'll need to fully configure and setup your key(s). The information on this page assumes you've already installed `GnuPG` and initialized your local keyring. 

!!! abstract inline end "Requirements"

    [:simple-github: GPGTools/pinentry](https://github.com/GPGTools/pinentry)

    [:simple-homebrew: pinentry-mac](https://formulae.brew.sh/formula/pinentry-mac)
    
    `brew install pinentry-mac`
### `pinentry-mac`

If you'll be using PINs with your Yubikey (and I strongly recommend you do), you'll want some method of entering them other than strictly through terminal. The default tool for working with Yubikey is `pinentry-curses` -- but a gui version is available through `pinentry-mac`.

```bash
$ brew install pinentry-mac

==> Fetching pinentry-mac
######################################################################## 100.0%
==> Pouring pinentry-mac--1.1.1.1.arm64_ventura.bottle.tar.gz

==> Caveats
You can now set this as your pinentry program like

~/.gnupg/gpg-agent.conf
    pinentry-program /opt/homebrew/bin/pinentry-mac

==> Summary
🍺  /opt/homebrew/Cellar/pinentry-mac/1.1.1.1: 19 files, 518.7KB
```

To complete the installation, we'll need to set `pinentry-mac` as the default tool. First, confirm the precise location by running command `where pinentry-mac` from the terminal.  Assuming the answer is the same default location as specified by the Homebrew output above:
```bash
# open (or create if it doesn't exist) ~/.gnupg/gpg-agent.conf
# and add the following line:
pinentry-program /opt/homebrew/bin/pinentry-mac
# save and close gpg-agent.conf

# reload the gpg-agent with the following command:
$ gpg-connect-agent reloadagent /bye
```

---

!!! abstract inline end "Requirements"

    [:simple-github: Yubico/yubikey-manager](https://github.com/Yubico/yubikey-manager)

    [:simple-yubico: yubikey-manager](https://developers.yubico.com/yubikey-manager/)

    [:simple-homebrew: ykman](https://formulae.brew.sh/formula/ykman#default)
    
    `brew install ykman`
### `yubikey-manager` (`ykman`)

Yubikey Manager and it's CLI counterpart, `ykman`, are tools for configuring Yubikey *(FIDO2, OTP, PIV, identify serial and firmware, enable or disable interfaces, and other extended features)*. 

The GUI version (Yubikey Manager) is occasionally helpful for quick tasks or checking information, but the majority of the time I use `ykman`. The GUI only exposes some configuration control, while the CLI tool provides full access to every feature.

See also: [`ykman` CLI Documentation](https://docs.yubico.com/software/yubikey/tools/ykman/Using_the_ykman_CLI.html)

``` bash
$ ykman --help

Usage: ykman [OPTIONS] COMMAND [ARGS]...

  Configure your YubiKey via the command line.

  Examples:

    List connected YubiKeys, only output serial number:
    $ ykman list --serials

    Show information about YubiKey with serial number 123456:
    $ ykman --device 123456 info

Options:
  -d, --device SERIAL             specify which YubiKey to interact with by serial number
  -r, --reader NAME               specify a YubiKey by smart card reader name (can't be used with --device or list)
  -t, --scp-ca FILENAME           specify the CA to use to verify the SCP11 card key (CA-KLCC)
  -s, --scp CRED                  specify private key and certificate chain for secure messaging, can be used multiple times to provide key and certificates in multiple files (private key,
                                  certificates in leaf-last order), OR SCP03 keys in hex  separated by colon (:) K-ENC:K-MAC[:K-DEK]
  -p, --scp-password PASSWORD     specify a password required to access the --scp file, if needed
  -l, --log-level [ERROR|WARNING|INFO|DEBUG|TRAFFIC]
                                  enable logging at given verbosity level
  --log-file FILE                 write log to FILE instead of printing to stderr (requires --log-level)
  --diagnose                      show diagnostics information useful for troubleshooting
  -v, --version                   show version information about the app
  --full-help                     show --help output, including hidden commands
  -h, --help                      show this message and exit

Commands:
  info     show general information
  list     list connected YubiKeys
  script   run a python script
  config   configure the YubiKey, enable or disable applications
  fido     manage the FIDO applications
  hsmauth  manage the YubiHSM Auth application
  oath     manage the OATH application
  openpgp  manage the OpenPGP application
  otp      manage the YubiOTP application
  piv      manage the PIV application
```

---

!!! abstract inline end "Requirements"

    [:simple-github: Yubico/yubico-piv-tool](https://github.com/Yubico/yubico-piv-tool)

    [:simple-yubico: yubico-piv-tool](https://developers.yubico.com/yubico-piv-tool/)

    [:simple-homebrew: yubico-piv-tool](https://formulae.brew.sh/formula/yubico-piv-tool)
    
    `brew install yubico-piv-tool`
### `yubico-piv-tool`

The Yubico PIV Tool is a CLI tool for interacting with the Personal Identity Verification (PIV) application on a YubiKey. With it, you can generate keys on the device, import keys and certificates, and create certificate requests. 

``` bash
$ yubico-piv-tool --help   

Usage: yubico-piv-tool [OPTION]...

  -h, --help               Print help and exit
      --full-help          Print help, including hidden options, and exit
  -V, --version            Print version and exit
  -v, --verbose[=INT]      Print more information  (default=`0')
  -r, --reader=STRING      Only use a matching reader  (default=`Yubikey')
  -k, --key[=STRING]       Management key to use, if no value is specified key
                             will be asked for
                             (default=`010203040506070801020304050607080102030405060708')
  -a, --action=ENUM        Action to take  (possible values="version",
                             "generate", "set-mgm-key", "reset",
                             "pin-retries", "import-key",
                             "import-certificate", "set-chuid",
                             "request-certificate", "verify-pin",
                             "verify-bio", "change-pin", "change-puk",
                             "unblock-pin", "selfsign-certificate",
                             "delete-certificate", "read-certificate",
                             "status", "test-signature", "test-decipher",
                             "list-readers", "set-ccc", "write-object",
                             "read-object", "attest", "move-key",
                             "delete-key")

       Multiple actions may be given at once and will be executed in order
       for example --action=verify-pin --action=request-certificate

  -s, --slot=ENUM          What key slot to operate on  (possible
                             values="9a", "9c", "9d", "9e", "82",
                             "83", "84", "85", "86", "87", "88",
                             "89", "8a", "8b", "8c", "8d", "8e",
                             "8f", "90", "91", "92", "93", "94",
                             "95", "f9")

       9a is for PIV Authentication
       9c is for Digital Signature (PIN always checked)
       9d is for Key Management
       9e is for Card Authentication (PIN never checked)
       82-95 is for Retired Key Management
       f9 is for Attestation

      --to-slot=ENUM       What slot to move an existing key to  (possible
                             values="9a", "9c", "9d", "9e", "82",
                             "83", "84", "85", "86", "87", "88",
                             "89", "8a", "8b", "8c", "8d", "8e",
                             "8f", "90", "91", "92", "93", "94",
                             "95", "f9")

       9a is for PIV Authentication
       9c is for Digital Signature (PIN always checked)
       9d is for Key Management
       9e is for Card Authentication (PIN never checked)
       82-95 is for Retired Key Management
       f9 is for Attestation

  -A, --algorithm=ENUM     What algorithm to use  (possible values="RSA1024",
                             "RSA2048", "RSA3072", "RSA4096",
                             "ECCP256", "ECCP384", "ED25519", "X25519"
                             default=`RSA2048')
  -H, --hash=ENUM          Hash to use for signatures  (possible
                             values="SHA1", "SHA256", "SHA384",
                             "SHA512" default=`SHA256')
  -n, --new-key=STRING     New management key to use for action set-mgm-key, if
                             omitted key will be asked for
      --pin-retries=INT    Number of retries before the pin code is blocked
      --puk-retries=INT    Number of retries before the puk code is blocked
  -i, --input=STRING       Filename to use as input, - for stdin  (default=`-')
  -o, --output=STRING      Filename to use as output, - for stdout
                             (default=`-')
  -K, --key-format=ENUM    Format of the key being read/written  (possible
                             values="PEM", "PKCS12", "GZIP", "DER",
                             "SSH" default=`PEM')
      --compress           Compress a large certificate using GZIP before
                             import  (default=off)
      --global             Reset the whole device over all applications
                             (default=off)
  -p, --password=STRING    Password for decryption of private key file, if
                             omitted password will be asked for
  -S, --subject=STRING     The subject to use for certificate request

       The subject must be written as:
       /CN=host.example.com/OU=test/O=example.com/

      --serial=INT         Serial number of the self-signed certificate
      --valid-days=INT     Time (in days) until the self-signed certificate
                             expires  (default=`365')
  -P, --pin=STRING         Pin/puk code for verification, if omitted pin/puk
                             will be asked for
  -N, --new-pin=STRING     New pin/puk code for changing, if omitted pin/puk
                             will be asked for
      --pin-policy=ENUM    Set pin policy for action generate or import-key.
                             Only available on YubiKey 4 or newer  (possible
                             values="never", "once", "always",
                             "matchonce", "matchalways")
      --touch-policy=ENUM  Set touch policy for action generate, import-key or
                             set-mgm-key. Only available on YubiKey 4 or newer
                             (possible values="never", "always",
                             "cached")
      --id=INT             Id of object for write/read object
  -f, --format=ENUM        Format of data for write/read object  (possible
                             values="hex", "base64", "binary"
                             default=`hex')
      --attestation        Add attestation cross-signature  (default=off)
  -m, --new-key-algo=ENUM  New management key algorithm to use for action
                             set-mgm-key  (possible values="TDES",
                             "AES128", "AES192", "AES256" default=`TDES')
      --scp11              Use encrypted communication as specified by Secure
                             Channel Protocol 11 (SCP11b)  (default=off)
```

---

!!! abstract inline end "Requirements"

    [:simple-github: Yubico/yubioath-flutter](https://github.com/Yubico/yubioath-flutter)

    [:simple-yubico: Yubico Authenticator](https://www.yubico.com/products/yubico-authenticator/)

    [:simple-homebrew: yubico-authenticator](https://formulae.brew.sh/cask/yubico-authenticator#default)
    
    `brew install --cask yubico-authenticator`
### `Yubico Authenticator`

Yubico Authenticator has evolved into a more holistic app for configuring Yubikey, but its primary purpose is serving up OATH codes saved to Yubikey. 

Outside of the OATH functionality, it provides a clean UI for identifying serial number and firmware version, configuring the two touch slots, and reviewing saved Passkeys and PIV certificates.

See also: [Yubico Authenticator User Guide](https://docs.yubico.com/software/yubikey/tools/authenticator/auth-guide/webdocs.pdf)

---

## Yubikey Configuration Resources

- [YubiKey Technical Manual](https://docs.yubico.com/hardware/yubikey/yk-tech-manual/webdocs.pdf)
- [ykman CLI and YubiKey Manager GUI Guide](https://docs.yubico.com/software/yubikey/tools/ykman/webdocs.pdf)

---

## Working With Multiple Keys

It's always recommended to have at least one spare Yubikey to configure at the same time you sit down to configure your first key. What you don't want to happen is to spend a few hours getting your key configured just how you want it, but then decide after-the-fact that you should have purchased a spare.  When that spare arrives, it will be frustrating to try to get that second key configured exactly as you did the first key unless you wrote everything down and saved all of the passcodes, pins, etc. in your password manager. 

Worse, if you've only setup a single key and then lose it, you're going to have to spend potentially hours going through all of your sites and services to remove your old key, setup a new one, or add alternative / backup authentication methods. 

When you do begin to work with multiple keys, you can connect each of them to your computer to setup simultaneously without having to constantly connect-disconnect-reconnect your keys a hundred times. The solution to this is noting the Serial Numbers for each of your keys.

!!! success "Setup Order"

    For GnuPG operations, you'll need to work on a single connected Yubikey at a time. For this reason, I always start with the OpenGPG configuration of each Yubikey first before moving on to the rest of the function setups (which can handle all cards being connected at once).


``` bash
# multiple keys connected
$ ykman info 

ERROR: Multiple YubiKeys detected. Use --device SERIAL to specify which one to use.


# identify all of the keys that are connected
$ ykman list

YubiKey 5C NFC (5.7.1) [OTP+FIDO+CCID] Serial: 12121212
YubiKey 5C Nano (5.7.1) [OTP+FIDO+CCID] Serial: 31313131
YubiKey 5C Nano (5.7.1) [OTP+FIDO+CCID] Serial: 32323232
YubiKey 5C NFC (5.7.1) [OTP+FIDO+CCID] Serial: 13131313
```

To specify a device, `--device 123456` must always come immediately after invocation of the CLI tool:

``` bash
$ ykman --device 12121212 info

Device type: YubiKey 5C NFC
Serial number: 12121212
Firmware version: 5.7.1
Form factor: Keychain (USB-C)
Enabled USB interfaces: OTP, FIDO, CCID
NFC transport is enabled

Applications	USB    	NFC    
Yubico OTP  	Enabled	Enabled
FIDO U2F    	Enabled	Enabled
FIDO2       	Enabled	Enabled
OATH        	Enabled	Enabled
PIV         	Enabled	Enabled
OpenPGP     	Enabled	Enabled
YubiHSM Auth	Enabled	Enabled



$ ykman --device 31313131 info

Device type: YubiKey 5C Nano
Serial number: 31313131
Firmware version: 5.7.1
Form factor: Nano (USB-C)
Enabled USB interfaces: OTP, FIDO, CCID

Applications
Yubico OTP  	Enabled
FIDO U2F    	Enabled
FIDO2       	Enabled
OATH        	Enabled
PIV         	Enabled
OpenPGP     	Enabled
YubiHSM Auth	Enabled
```

---

## PINs and Security

When reviewing the various modes available on Yubikey (section below), keep in mind that each one has it's own PIN security preference settings. Some functions require the PIN to be set while others do not. Additionally, some OSes place additional requirements on PIN type or length above and beyond the minimum supported by Yubico. 

For a medium baseline of security, make sure your PINs are at least 6 digits where fewer are supported.

The strength of a Yubikey's security is derived from two of the three authentication factors: something you ***have***, and something you ***know***. The stronger your PINs are, the more secure your Yubikey is. Enabling optional features that require PINs, passwords, or touch increase the security of your device at the cost of inconvenience. 

| Yubikey App / PIN | PIN Length | Default | Set/Change<br />Required | Options | Notes |
| ----------- |:---:| ------- |:--------:| ------- | ----- |
| FIDO2<br />*(Passkeys)* | 4 to 63 | Not Set | Yes | - Always Require User Verification *(User Presence)*<br />- Change Minimum PIN Length<br /> | - Charset: 0-9, a-z, A-Z, (no special characters) | n/a | 
| OTP<br />*(Touch slots)* | n/a | short touch /<br />long touch | No | - Configure as: Yubico OTP, Challenge-response, Static password, or OATH-HOTP | - No PIN settings. OTP operates on touch only. |
| OATH | Unrestricted | Not Set | No | - Set / Change a password<br />- Remember password on this computer<br />- Forget / Clear password  | - Charset: 0-9, a-z, A-Z, special characters allowed<br />- Password not required by default, anyone with physical access to yubikey can generate codes. |
| OpenPGP<br />Access PIN | 6 to 64 | `123456` | No | - Change PIN<br />- Change max attempts allowed<br />- Require touch per PGP subkey | - Charset: alphanumeric only<br />- Used for day-to-day PGP activity<br />- Changing default not required but strongly advised. |
| OpenPGP<br />Admin PIN | 8 to 64 | `12345678` | No | - Change PIN<br />- Change max attempts allowed | - Charset: alphanumeric only<br />- Used for configuring PGP features<br />- Changing default not required but strongly advised. |
| OpenPGP<br />Reset PIN | 6 to 64 | Not Set | No | - Set / Change PIN<br />- Change max attempts allowed | - Charset: alphanumeric only<br />- Not required, but if set must be used to reset PGP app when max retries exceeded. Useful for allowing a user to reset PGP without giving them the Admin PIN for configuration access. |
| PIV<br />Access PIN | 6 to 8 | `123456` | No | - Change PIN<br />- Change max attempts allowed | - Charset: any ASCII, but OSes may limit to numeric only<br />- Used for day-to-day PIV activity<br />- Changing default not required but strongly advised. |
| PIV<br />Unlock PIN | 6 to 8 | `12345678` | No | - Change PIN<br />- Change max attempts allowed | - Charset: any ASCII, but OSes may limit to numeric only<br />- Used for unlocking the PIN if max retries exceeded<br />- Changing default not required but strongly advised. |
| PIV<br />Mgmt Key | depends on algorithm used | see note below | No | - Supports 3DES, or<br />AES 128/192/256 | - Required for management functions. |


For the PIV Management Key: 

- The default Management Key is `010203040506070801020304050607080102030405060708` 3DES (also referred to as *TDES*) and 48 characters long. Other acceptable formats are AES 128 (32 characters), AES 192 (48 characters), and AES 256 (64 characgers). Keys can be randomly generated on-device or manually entered. 
- The Management Key can optionally be protected by PIN.


### Attemps and Retries

In general, if you exceed the retry count for a specified application, that application will become fully blocked from operation and cannot be used again until an "app reset" has been performed. This causes all data for the blocked app to be deleted. 

| Yubikey App | Default Retries | Changable | Notes |
| ----------- |:---------------:|:---------:| ----- |
| FIDO2 | 8 | No | Three (3) incorrect attempts in a row: key must be removed and reinserted.<br />Eight (8) attempts: FIDO2 is disabled entirely and FIDO2 app must be reset, wiping all FIDO credentials from Yubikey. |
| OTP | n/a | n/a | Touch only. |
| OATH | no enforced limit | No | Forgotten passwords can only be 'reset' by resetting the entire OATH application on Yubikey, effectively wiping all OATH codes from the device. |
| OpenPGP | 3 / 3 / 3 | Yes | If you exceed the *access PIN* retry count, OpenPGP becomes "**locked**" until you enter your *admin PIN*.<br />If you've set a *reset PIN*, the *reset PIN* must be used to unlock Yubikey instead.<br />If you exceed the *admin PIN* or *reset PIN* retry count, OpenPGP becomes "**blocked**" and cannot be used until OpenPGP is reset, wiping all OpenPGP keys from Yubikey. |
| PIV | 3 / 3 | Yes | If *PIN* retry count is exceeded, PIV becomes "**locked**" until you enter your *PUK*.<br />If the *PUK* retry count is exceeded, PIV becomes "**blocked**" and cannot be used until PIV is reset, wiping all PIV data from Yubikey, including PIN, PUK, and Management Key. |

---

## Apps (modes of operation)

### FIDO U2F / FIDO2 

*(Passwordless & 2FA Authentication)*

* Used for passwordless login or as a second factor for online accounts.
* Works with services like Microsoft, Google, GitHub, and Facebook.
* Resistant to phishing since authentication only works on legitimate sites.
* FIDO2 enables single-factor authentication, while U2F (universal second factor) provides two-factor.

***Use-case example:*** Logging into Microsoft with just your Yubikey and PIN, replacing username and password.

***Capacity:*** `100 resident (discoverable) credentials` (firmware >= 5.7), `25 credentials` (firmware < 5.7)


### OTP (Touch Slots)

* Generates one-time passwords (OTP) for login security.
* Works with legacy systems that don't support modern authentication (like older VPNs).
* Supports both Yubico OTP (proprietary, cloud-verified) and HOTP (event-based OTP).

***Use-case example:*** Logging into a VPN that requires a one-time password along with your username and password. 

***Use-case example:*** Programming the long-touch slot as a 'Static Password' and assigning a very long complex password.

***Capacity:*** `2 slots`

The easiest way to setup one or both of the OTP slots on Yubikey is to use the "`Yubikey Manager`" or "`Yubico Authenticator`" apps. The OTP functionality is accessed by either short-pressing or long-pressing the Yubico logo on the physical key.


### OATH (TOTP & HOTP)

* Works exactly like Google Authenticator and Authy but stores the codes securely on your Yubikey - not on your computer or mobile device. 
* Generates TOTP (time-based one-time passwords) for 2FA logins.
* Requires the Yubikey to generate authentication codes, reading only the datetime from your computer or mobile device. 

***Use-case example:*** Using the Yubico Authenticator app to generate 6-digit TOTP codes for logging into services like GitHub, Amazon, or your bank account.

***Capacity:*** `64 slots` (firmware >= 5.7), `32 slots` (firmware < 5.7)


### OpenPGP 

*(Encryption, Signing, and Authentication)*

*(If you've never created PGP keys before and want a walk-through of this process, see the [PGP Primer](../pgp/pgp.md#creating-your-keys))*

* Stores PGP private keys securely on the Yubikey itself. 
* Used for encrypting/decrypting emails, signing files, and SSH authentication.
* Ensures security of private keys - keys cannot be exported once loaded into Yubikey.

***Use-case example:*** Signing and encrypting emails with OpenPGP to ensure only the intended recipient can read them. 

***Use-case example:*** Signing code commits on GitHub.

***Capacity:*** `3 subkeys` (Encryption, Signature, Authentication) 


### PIV (Smart Card)

* Functions as a hardware smart card for identity verification.
* Used for logging into computers securely.
* Supports digital signing and encryption for documents.
* Ensures security of private keys - keys cannot be exported once loaded into Yubikey.

***Capacity:*** `19` functionally usable slots (`5` specific-function, `14` retired key slots).