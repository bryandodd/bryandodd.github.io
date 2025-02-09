---
title: Yubikey Prerequisites
icon: simple/yubico
---

# Yubikey Setup & Configuration

!!! info "Annotations"

     Annotations are used throughout some of the examples below. Clicking the <span style="color: yellow">:octicons-feed-plus-16:{ title="annotation icon" }</span> symbols reveals additional comments intended for clarity.

## Requirements

There are a few prerequisites you'll need to setup before continuing on to setup your key(s). The information on this page assumes you've already installed `gnupg` and initialized your local keyring. 

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

Yubikey Manager is a CLI tool for configuring your Yubikey device: FIDO2, OTP, PIV, identify serial and firmware, enable or disable interfaces, and other extended features of Yubikey devices. This is the tool I use most frequently when interacting with my device.

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
