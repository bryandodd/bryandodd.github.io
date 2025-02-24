---
title: Yubikey OpenPGP Configuration
---

# Yubikey OpenPGP Configuration

For more information about PGP *(you'll also see it referred to as "GPG")*, see the [PGP Primer](../pgp/pgp.md). You'll need to have your Keys and Secret Keys handy to successfully store them on your Yubikey. 

## Expected / Assumed Configuration

```
gpg --list-secret-keys --with-subkey-fingerprint

[keyboxd]
---------
sec#  ed25519 YYYY-MM-DD [C]
      74BD90B590F9AA984391707383308CA85B65951C
uid           [ultimate] Bryan Dodd <bryan@dodd.dev>
ssb>  ed25519 YYYY-MM-DD [S]
      ................................5400D8FF
      Card serial no. = 0006 23456789
ssb>  cv25519 YYYY-MM-DD [E]
      ................................B4C98A69
      Card serial no. = 0006 23456789 
ssb>  ed25519 YYYY-MM-DD [A]
      ................................EB5285E8
      Card serial no. = 0006 23456789 
```

Yubikey's conventions expect that you have a master certifying key `[C]` from which three (3) subkeys have been derived:

* A signing key `[S]`
* An encryption key `[E]`
* An authentication key `[A]`

In the above example:

* sec# : `sec` stands for Secret Key (in this case a master certifying key). Ideally you do not want this key to reside on the machine you use daily because the only time it is ever needed is when generating new subkeys or revoking them. The `#` symbold indicates that the machine these keys are associated with is aware that a secret certifying key exists, but it does not exist on the machine. If this key were present, there would be no additional symbol displayed - it would simply read `sec`.

* ssb> : `ssb` stands for Subkey. As with `sec` above, `ssb` by itself indicates that the subkey's material is actually present and saved on the local machine's keyring. The `>` symbol means that the machine is aware of the subkey and knows where to get it, but does not have a copy of the key material saved locally on the machine. This is often referred to as a `key stub`. You will this referred to as "stubbing out keys" or "the keys have been stubbed (out)."

* Directly under the `sec` key information is `uid`. This is the key identity. Because this is my key, the secret key is marked with `[ultimate]` trust. This means that any operations relying on this key should assume that it has ultimate authority and no additional prompts need to be shown to the user. How much trust you place in a key is entirely subjective. It is up to individual users to determine how much they trust a key - this is referred to as the [Web of Trust](https://en.wikipedia.org/wiki/Web_of_trust).

Trust levels:

* `unknown`: The default trust level, where there is not enough information to discern the trust of a key.
* `never`: You've explicitly marked this key so it is ***never*** trusted. You might use this if you know the key has been compromised or being used by a threat actor.
* `marginal`: Marginal means "they're good, but not *too* good." GnuPG will mark a key as `trusted` when it has received signatures from at least three (3) other keys that you have granted at least `marginal` trust to. (See "Web of Trust" link above).
* `full`: Should only be used with keys you actually trust. Unlike `marginal` trust, `full` trust only requires one signature from a key you've marked as `trusted`. Think of this as a "vouching" system. 
    * e.g.: John trusts Mark's key. Mark signs Paul's key. John now trusts Paul's key because Mark is `trusted` and trusts (vouched) for Paul. 
* `ultimate`: Should really only ever be used with your own keys because it is the highest level of trust that can be assigned. 

## Getting Started

Begin by ensuring that only your new Yubikey is connected to your machine, and execute `gpg --card-status` and `ykman info` to verify that you're targeting the correct device. 

Though *some* of the initial steps can be done with the YubiKey Manager GUI app, it doesn't support or include everything you need to configure OpenPGP with your Yubikey, so I recommend sticking with the command-line tools for this setup.

``` bash
$ gpg --card-status

Reader ...........: Yubico YubiKey OTP FIDO CCID
Application ID ...: D1234567891234567891234567891234
Application type .: OpenPGP
Version ..........: 3.4
Manufacturer .....: Yubico
Serial number ....: 23456789
Name of cardholder: [not set]
Language prefs ...: [not set]
Salutation .......: 
URL of public key : [not set]
Login data .......: [not set]
Signature PIN ....: not forced
Key attributes ...: rsa2048 rsa2048 rsa2048
Max. PIN lengths .: 127 127 127
PIN retry counter : 3 0 3
Signature counter : 0
KDF setting ......: off
UIF setting ......: Sign=off Decrypt=off Auth=off
Signature key ....: [none]
Encryption key....: [none]
Authentication key: [none]
General key info..: [none]


$ ykman info

Device type: YubiKey 5C NFC
Serial number: 23456789
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
```

!!! important "Default PINs"

    If this is a new key and you haven't modified the default PINs, make note of them below. We'll be changing them shortly.  I strongly recommend making a note of all of your pins and passphrases in a secure password manager to help ensure you don't lose access to your Yubikey - especially if you won't use them often.
    
    - **PIN: `123456`**
    - **Admin PIN: `12345678`**

## Update PINs

> Admin PIN must be set before Reset Code

``` bash
# Set the OpenPGP PIN (min 6 alphanumeric)
ykman openpgp access change-pin --pin 123456 --new-pin XXXXXX
User PIN has been changed.

# Set the OpenPGP Admin PIN (min 8 alphanumeric)
ykman openpgp access change-admin-pin --admin-pin 12345678 --new-admin-pin 88888888
Admin PIN has been changed.

# Set the OpenPGP Reset Code (min 8 alphanumeric)
ykman openpgp access change-reset-code --admin-pin 88888888 --reset-code 77777777
Reset Code has been changed.


# Set retry count for PIN, Reset Code, and Admin PIN (optional!)
ykman openpgp access set-retries 6 6 6
    Enter Admin PIN: 88888888
    Set PIN retry counters to: 6 6 6? [y/N]: y
    Number of PIN/Reset Code/Admin PIN retries set.
```

Here, we're changing the default retry / attempt counts from their default `3` to `6`.

## Update Operational Settings

``` bash
ykman openpgp keys set-touch --help   
Usage: ykman openpgp keys set-touch [OPTIONS] KEY POLICY

  Set the touch policy for OpenPGP keys.

  The touch policy is used to require user interaction for all operations using the private key on the YubiKey. The touch policy is set individually for each key slot. To see the current touch
  policy, run the "openpgp info" subcommand.

  Touch policies:

  Off (default)   no touch required
  On              touch required
  Fixed           touch required, can\'t be disabled without deleting the private key
  Cached          touch required, cached for 15s after use
  Cached-Fixed    touch required, cached for 15s after use, can't be disabled
                  without deleting the private key

  KEY     key slot to set (sig, dec, aut or att)
  POLICY  touch policy to set (on, off, fixed, cached or cached-fixed)

Options:
  -a, --admin-pin TEXT  Admin PIN for OpenPGP
  -f, --force           confirm the action without prompting
  -h, --help            show this message and exit


# Set touch requirements for signing, decryption, authentication, and attestation
ykman openpgp keys set-touch sig off
    Enter Admin PIN: 
    Set touch policy of SIG key to off? [y/N]: y
    Touch policy for slot SIG set.

ykman openpgp keys set-touch dec off
    Enter Admin PIN: 
    Set touch policy of DEC key to off? [y/N]: y
    Touch policy for slot DEC set.

ykman openpgp keys set-touch aut off
    Enter Admin PIN: 
    Set touch policy of AUT key to off? [y/N]: y
    Touch policy for slot AUT set.

ykman openpgp keys set-touch att cached
    Enter Admin PIN: 
    Set touch policy of ATT key to cached? [y/N]: y
    Touch policy for slot ATT set.
```

In the commands above, I've set the "touch" requirements for signing, encryption (and decryption), and authenticating to `off`, but I've set attestation to `cached`. 

| Value | Meaning |
| ----- | ------- |
| `Off` *(default)* | no touch required |
| `On` | touch required |
| `Fixed` | touch required, can't be disabled without deleting the private key |
| `Cached` | touch required, cached for 15s after use |
| `Cached-Fixed` | touch required, cached for 15s after use, can't be disabled<br />without deleting the private key |

## Confirm Touch Settings

``` bash
$ ykman openpgp info

OpenPGP version:            3.4
Application version:        5.7.1
PIN tries remaining:        6
Reset code tries remaining: 6
Admin PIN tries remaining:  6
Require PIN for signature:  Once
KDF enabled:                False
Touch policies:            
  Signature key:      Off
  Encryption key:     Off
  Authentication key: Off
  Attestation key:    Cached
```

## Update Yubikey PGP Properties

``` bash
$ gpg --card-edit

Reader ...........: Yubico YubiKey OTP FIDO CCID
Application ID ...: D1234567891234567891234567891234
Application type .: OpenPGP
Version ..........: 3.4
Manufacturer .....: Yubico
Serial number ....: 23456789
Name of cardholder: [not set]
Language prefs ...: [not set]
Salutation .......: 
URL of public key : [not set]
Login data .......: [not set]
Signature PIN ....: not forced
Key attributes ...: rsa2048 rsa2048 rsa2048
Max. PIN lengths .: 127 127 127
PIN retry counter : 6 6 6
Signature counter : 0
KDF setting ......: off
UIF setting ......: Sign=off Decrypt=off Auth=off
Signature key ....: [none]
Encryption key....: [none]
Authentication key: [none]
General key info..: [none]

gpg/card> help
quit           quit this menu
admin          show admin commands
help           show this help
list           list all available data
fetch          fetch the key specified in the card URL
passwd         menu to change or unblock the PIN
verify         verify the PIN and list all data
unblock        unblock the PIN using a Reset Code
openpgp        switch to the OpenPGP app

gpg/card> admin
Admin commands are allowed

gpg/card> help
quit           quit this menu
admin          show admin commands
help           show this help
list           list all available data
name           change card holder's name
url            change URL to retrieve key
fetch          fetch the key specified in the card URL
login          change the login name
lang           change the language preferences
salutation     change card holder's salutation
cafpr          change a CA fingerprint
forcesig       toggle the signature force PIN flag
generate       generate new keys
passwd         menu to change or unblock the PIN
verify         verify the PIN and list all data
unblock        unblock the PIN using a Reset Code
factory-reset  destroy all keys and data
kdf-setup      setup KDF for PIN authentication (on/single/off)
key-attr       change the key attribute
uif            change the User Interaction Flag
openpgp        switch to the OpenPGP app

gpg/card> name
Cardholder's surname: Dodd
Cardholder's given name: Bryan

gpg/card> lang
Language preferences: en

gpg/card> login
Login data (account name): bryan@dodd.dev

gpg/card> url
URL to retrieve public key: https://keys.openpgp.org/vks/v1/by-fingerprint/74BD90B590F9AA984391707383308CA85B65951C

gpg/card> list

Reader ...........: Yubico YubiKey OTP FIDO CCID
Application ID ...: D1234567891234567891234567891234
Application type .: OpenPGP
Version ..........: 3.4
Manufacturer .....: Yubico
Serial number ....: 23456789
Name of cardholder: Bryan Dodd
Language prefs ...: en
Salutation .......: 
URL of public key : https://keys.openpgp.org/vks/v1/by-fingerprint/74BD90B590F9AA984391707383308CA85B65951C
Login data .......: bryan@dodd.dev
Signature PIN ....: not forced
Key attributes ...: rsa2048 rsa2048 rsa2048
Max. PIN lengths .: 127 127 127
PIN retry counter : 6 6 6
Signature counter : 0
KDF setting ......: off
UIF setting ......: Sign=off Decrypt=off Auth=off
Signature key ....: [none]
Encryption key....: [none]
Authentication key: [none]
General key info..: [none]

gpg/card> quit
```

Not every property is required to be set, but setting as much as you can is helpful in different circumstances. 

For example, setting the URL enables use of the `--fetch` command to automatically retrieve a copy of your public key.

## Loading PGP Keys

!!! warning "Plan ahead before continuing ..."

    Keys can ***ONLY*** be transferred to Yubikey from your ***GnuPG Keyring***.

    * If you've previously loaded them to another Yubikey, all you have in your current keyring will be key *stubs*. This is not sufficient for setting up a new Yubikey, so additional steps will be required.
    * Make sure you've got your public / private keys exported to ASCII-armored files. You will need to re-load your keys from backup to replace stubs with actual keys.
    * Loading keys to Yubikey is a <span style="color: var(--md-code-hl-special-color)">***one way operation***</span>. Once loaded into Yubikey, you can't export your private key again, so your only option is to delete the keys and upload new ones.

    **This step is especially important.** If you intend to load your OpenPGP keys onto ***multiple Yubikeys***, pay close attention to the order of operations performed here.

To begin, the keys you want to transfer to Yubikey must be loaded to your GnuPG Keyring. If your keys are *not currently loaded and accessible* from your local keyring, you must first re-load them. The most likely reason you may be in this situation is if you've previously loaded your keys to another Yubikey and are setting up a second or new Yubikey. 

When you issue the command to move your keys to Yubikey, the keys are *literally* moved. There is no option to only "copy" them. The only thing remaining in your local keyring after the transfer process is a ***stub*** (pointer to the Yubikey you moved them to), and the process cannot be reversed.

---

To determine if your keys are physically present on your machine or if they've been moved elsewhere, run `gpg --list-secret-keys`. The subkeys will be listed at the bottom as `ssb`.  If you see the `>` symbol, this indicates you only have stubs loaded. You will not be able to transfer your keys to Yubikey in this state.

To remedy this, you must have a previously exported set of your private keys available to you - whether thats on your local filesystem, a USB drive, a network drive, etc. If you followed along with the key generation in the [PGP Primer](../pgp/pgp.md#creating-your-keys), you should have a copy of your subkeys saved as `subkeys.asc` (or similar). We'll need these soon.

``` bash
$ gpg --list-secret-keys --with-keygrip

[keyboxd]
---------
sec#  ed25519 YYYY-MM-DD [C]
      74BD90B590F9AA984391707383308CA85B65951C
      Keygrip = ...A47E26C0196ED67BD
uid           [ultimate] Bryan Dodd <bryan@dodd.dev>
ssb>  ed25519 YYYY-MM-DD [S]    # Note the ">" indicating that only stubs exist locally.
      Keygrip = ...BA272DF1
ssb>  cv25519 YYYY-MM-DD [E]
      Keygrip = ...CC740E41
ssb>  ed25519 YYYY-MM-DD [A]
      Keygrip = ...E566D05C
```

The `Keygrip` value from the output indicates where your key stubs are stored locally. Take extra care not to delete the wrong files while you <span style="color: var(--md-code-hl-special-color)">***delete***</span> the keygrip files for the subkeys you previously stubbed: **`~/.gnupg/private-keys-v1.d/<keygrip>.key`**.

With the keygrip files removed, import the subkeys from your backup `.asc` file:

``` bash
$ gpg --import subkeys.asc

gpg: key 83308CA85B65951C: "Bryan Dodd <bryan@dodd.dev>" not changed
gpg: To migrate 'secring.gpg', with each smartcard, run: gpg --card-status
gpg: key 83308CA85B65951C: secret key imported
gpg: Total number processed: 1
gpg:              unchanged: 1
gpg:       secret keys read: 1
gpg:   secret keys imported: 1
```

If your master key is passphrase protected, be prepared to enter it during the import process - even if your master key is not currently saved to the computer you're working from. Once complete, enumerate your secret keys and note that the stub identifier (`>`) is no longer present, indicating the key material is indeed present on your local keyring again.

``` bash
$ gpg --list-secret-keys --with-subkey-fingerprint

[keyboxd]
---------
sec#  ed25519 YYYY-MM-DD [C]
      74BD90B590F9AA984391707383308CA85B65951C
uid           [ultimate] Bryan Dodd <bryan@dodd.dev>
ssb   ed25519 YYYY-MM-DD [S] # (1)
ssb   cv25519 YYYY-MM-DD [E]
ssb   ed25519 YYYY-MM-DD [A]
```

1.   :material-key-alert: The "`>`" is no longer present, signifying that the key material is present on the local keyring once again.

***We're now ready to copy the subkeys over to your new Yubikey.*** Like before, however, this is a one-way operation. Transferring the keys will once again replace the keys in your keyring with stubs. If this is your first time transferring private keys to Yubikey, read through the entire process carefully so you know what to expect. 

You're going to need to ...

* select a key
* move it to yubikey
* deselect the key
* select the next key
* etc., etc. 

... for each individual key. So pay close attention to where you are in the process to avoid mistakes. The final `save` step is necessary to save your work and complete the transfer.

``` bash
$ gpg --edit-key bryan@dodd.dev

gpg (GnuPG) 2.4.7; Copyright (C) 2024 g10 Code GmbH
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Secret subkeys are available.

pub  ed25519/83308CA85B65951C
     created: YYYY-MM-DD  expires: never       usage: C   
     trust: ultimate      validity: ultimate
ssb  ed25519/5B484E245400D8FF
     created: YYYY-MM-DD  expires: never       usage: S   
ssb  cv25519/FFEAEADFB4C98A69
     created: YYYY-MM-DD  expires: never       usage: E   
ssb  ed25519/2BA9D97DEB5285E8
     created: YYYY-MM-DD  expires: never       usage: A   
[ultimate] (1). Bryan Dodd <bryan@dodd.dev>

gpg> key 1 # (1)

pub  ed25519/83308CA85B65951C
     created: YYYY-MM-DD  expires: never       usage: C   
     trust: ultimate      validity: ultimate
ssb* ed25519/5B484E245400D8FF               # <== (8) 
     created: YYYY-MM-DD  expires: never       usage: S   
ssb  cv25519/FFEAEADFB4C98A69
     created: YYYY-MM-DD  expires: never       usage: E   
ssb  ed25519/2BA9D97DEB5285E8
     created: YYYY-MM-DD  expires: never       usage: A   
[ultimate] (1). Bryan Dodd <bryan@dodd.dev>

gpg> keytocard
Please select where to store the key:
   (1) Signature key
   (3) Authentication key
Your selection? 1 # (2)

# You may be prompted to enter your PASSPHRASE (if you set one for your master key)
# as well as your ADMIN PIN for every card action you take. 

pub  ed25519/83308CA85B65951C
     created: YYYY-MM-DD  expires: never       usage: C   
     trust: ultimate      validity: ultimate
ssb* ed25519/5B484E245400D8FF               # <== (9)
     created: YYYY-MM-DD  expires: never       usage: S   
ssb  cv25519/FFEAEADFB4C98A69
     created: YYYY-MM-DD  expires: never       usage: E   
ssb  ed25519/2BA9D97DEB5285E8
     created: YYYY-MM-DD  expires: never       usage: A   
[ultimate] (1). Bryan Dodd <bryan@dodd.dev>

Note: the local copy of the secret key will only be deleted with "save".
gpg> key 1 # (3)

pub  ed25519/83308CA85B65951C
     created: YYYY-MM-DD  expires: never       usage: C   
     trust: ultimate      validity: ultimate
ssb  ed25519/5B484E245400D8FF               # <== (13)
     created: YYYY-MM-DD  expires: never       usage: S   
ssb  cv25519/FFEAEADFB4C98A69
     created: YYYY-MM-DD  expires: never       usage: E   
ssb  ed25519/2BA9D97DEB5285E8
     created: YYYY-MM-DD  expires: never       usage: A   
[ultimate] (1). Bryan Dodd <bryan@dodd.dev>

gpg> key 2 # (4)

pub  ed25519/83308CA85B65951C
     created: YYYY-MM-DD  expires: never       usage: C   
     trust: ultimate      validity: ultimate
ssb  ed25519/5B484E245400D8FF
     created: YYYY-MM-DD  expires: never       usage: S   
ssb* cv25519/FFEAEADFB4C98A69               # <== (10)
     created: YYYY-MM-DD  expires: never       usage: E   
ssb  ed25519/2BA9D97DEB5285E8
     created: YYYY-MM-DD  expires: never       usage: A   
[ultimate] (1). Bryan Dodd <bryan@dodd.dev>

gpg> keytocard
Please select where to store the key:
   (2) Encryption key
Your selection? 2

# Again, be prepared to enter your PASSPHRASE and your ADMIN PIN

pub  ed25519/83308CA85B65951C
     created: YYYY-MM-DD  expires: never       usage: C   
     trust: ultimate      validity: ultimate
ssb  ed25519/5B484E245400D8FF
     created: YYYY-MM-DD  expires: never       usage: S   
ssb* cv25519/FFEAEADFB4C98A69               # <== (11)
     created: YYYY-MM-DD  expires: never       usage: E   
ssb  ed25519/2BA9D97DEB5285E8
     created: YYYY-MM-DD  expires: never       usage: A   
[ultimate] (1). Bryan Dodd <bryan@dodd.dev>

Note: the local copy of the secret key will only be deleted with "save".
gpg> key 2 # (5)

pub  ed25519/83308CA85B65951C
     created: YYYY-MM-DD  expires: never       usage: C   
     trust: ultimate      validity: ultimate
ssb  ed25519/5B484E245400D8FF
     created: YYYY-MM-DD  expires: never       usage: S   
ssb  cv25519/FFEAEADFB4C98A69               # <== (14)
     created: YYYY-MM-DD  expires: never       usage: E   
ssb  ed25519/2BA9D97DEB5285E8
     created: YYYY-MM-DD  expires: never       usage: A   
[ultimate] (1). Bryan Dodd <bryan@dodd.dev>

gpg> key 3 # (6)

pub  ed25519/83308CA85B65951C
     created: YYYY-MM-DD  expires: never       usage: C   
     trust: ultimate      validity: ultimate
ssb  ed25519/5B484E245400D8FF
     created: YYYY-MM-DD  expires: never       usage: S   
ssb  cv25519/FFEAEADFB4C98A69
     created: YYYY-MM-DD  expires: never       usage: E   
ssb* ed25519/2BA9D97DEB5285E8               # <== (12)
     created: YYYY-MM-DD  expires: never       usage: A   
[ultimate] (1). Bryan Dodd <bryan@dodd.dev>

gpg> keytocard
Please select where to store the key:
   (3) Authentication key
Your selection? 3

# Another prompt to enter your PASSPHRASE and your ADMIN PIN

pub  ed25519/83308CA85B65951C
     created: YYYY-MM-DD  expires: never       usage: C   
     trust: ultimate      validity: ultimate
ssb  ed25519/5B484E245400D8FF
     created: YYYY-MM-DD  expires: never       usage: S   
ssb  cv25519/FFEAEADFB4C98A69
     created: YYYY-MM-DD  expires: never       usage: E   
ssb* ed25519/2BA9D97DEB5285E8
     created: YYYY-MM-DD  expires: never       usage: A   
[ultimate] (1). Bryan Dodd <bryan@dodd.dev>

Note: the local copy of the secret key will only be deleted with "save".
gpg> save # (7)
```

1.  Enter `key 1` to SELECT the first subkey.
2.  The first subkey is our *signing* subkey, so take care to only select the option for the signature slot.
3.  You must enter `key 1` again to DESELECT the first subkey...
4.  ... and now enter `key 2` to SELECT the second subkey.
5.  As before, you must enter `key 2` again to DESELECT the second subkey...
6.  ... and enter `key 3` to SELECT the final subkey.
7.  You don't have to deselect the last subkey, but none of your changes will be SAVED until you enter `save` and press enter.
8.  The `*` symbol identifies the subkey you have *SELECTED*
9.  The `*` symbol identifies the subkey you have *SELECTED*
10.  The `*` symbol identifies the subkey you have *SELECTED*
11.  The `*` symbol identifies the subkey you have *SELECTED*
12.  The `*` symbol identifies the subkey you have *SELECTED*
13.  The key has been *DE-selected*.
14.  The key has been *DE-selected*.

## Verify Keys Have Moved

Let's confirm that your keys have once again been REMOVED from your local keyring and replaced with STUBS only:

``` bash
$ gpg --list-secret-keys

[keyboxd]
---------
sec#  ed25519 YYYY-MM-DD [C]
      74BD90B590F9AA984391707383308CA85B65951C
uid           [ultimate] Bryan Dodd <bryan@dodd.dev>
ssb>  ed25519 YYYY-MM-DD [S]    ## Note the ">" stub indicator has returned
ssb>  cv25519 YYYY-MM-DD [E]    ## signifiying that keys have been moved OUT
ssb>  ed25519 YYYY-MM-DD [A]    ## of the local keyring.
```

We can also use the `--with-subkey-fingerprint` to see the Yubikey serial number associated with our key:

``` bash
$ gpg --list-secret-keys --with-subkey-fingerprint

[keyboxd]
---------
sec#  ed25519 YYYY-MM-DD [C]
      74BD90B590F9AA984391707383308CA85B65951C
uid           [ultimate] Bryan Dodd <bryan@dodd.dev>
ssb>  ed25519 YYYY-MM-DD [S]
      ................................5400D8FF
      Card serial no. = 0006 23456789       ## <==
ssb>  cv25519 YYYY-MM-DD [E]
      ................................B4C98A69
      Card serial no. = 0006 23456789       ## <==
ssb>  ed25519 YYYY-MM-DD [A]
      ................................EB5285E8
      Card serial no. = 0006 23456789       ## <==
```

And finally, checking the Yubikey itself:

``` bash
$ gpg --card-status

Reader ...........: Yubico YubiKey OTP FIDO CCID
Application ID ...: D1234567891234567891234567891234
Application type .: OpenPGP
Version ..........: 3.4
Manufacturer .....: Yubico
Serial number ....: 23456789
Name of cardholder: Bryan Dodd
Language prefs ...: en
Salutation .......: 
URL of public key : https://keys.openpgp.org/vks/v1/by-fingerprint/74BD90B590F9AA984391707383308CA85B65951C
Login data .......: bryan@dodd.dev
Signature PIN ....: not forced
Key attributes ...: ed25519 cv25519 ed25519
Max. PIN lengths .: 127 127 127
PIN retry counter : 6 6 6
Signature counter : 0
KDF setting ......: off
UIF setting ......: Sign=off Decrypt=off Auth=off
Signature key ....: xxxx xxxx xxxx xxxx xxxx  xxxx xxxx xxxx 5400 D8FF
      created ....: YYYY-MM-DD 16:32:56
Encryption key....: xxxx xxxx xxxx xxxx xxxx  xxxx xxxx xxxx B4C9 8A69
      created ....: YYYY-MM-DD 16:36:40
Authentication key: xxxx xxxx xxxx xxxx xxxx  xxxx xxxx xxxx EB52 85E8
      created ....: YYYY-MM-DD 16:38:00
General key info..: sub  ed25519/5B484E245400D8FF YYYY-MM-DD Bryan Dodd <bryan@dodd.dev>
sec#  ed25519/83308CA85B65951C  created: YYYY-MM-DD  expires: never     
ssb>  ed25519/5B484E245400D8FF  created: YYYY-MM-DD  expires: never     
                                card-no: 0006 23456789
ssb>  cv25519/FFEAEADFB4C98A69  created: YYYY-MM-DD  expires: never     
                                card-no: 0006 23456789
ssb>  ed25519/2BA9D97DEB5285E8  created: YYYY-MM-DD  expires: never     
                                card-no: 0006 23456789
```

!!! info "Saving your keys to multiple Yubikeys ..."

    If you have multiple Yubikeys that you want to save your OpenPGP keys to, you'll have to repeat the steps on this page, starting with running the `gpg --list-secret-keys --with-keygrip` command to get the file path of your local key stubs. 

    After copying your keys to Yubikey, your local keys will be deleted every time. You must re-import your keys from your backup in order to move them to each Yubikey you want to store them on. 

    ***After your final Yubikey has been updated, do not restore your local keys from backup.*** Let them stay in your keyring as STUBS ONLY. This ensures that you can now only access your keys by having a Yubikey present.

## Which Yubikey to Use?

If you have multiple Yubikeys that you've configured with identical OpenPGP keys, note that the GnuPG Keyring will only ever remember the last Yubikey used to stub out your keys. 

Recall above that we used the `--with-subkey-fingerprint` command to get the fingerprints of our subkeys, but also to view the Yubikey serial number the keyring is expecting to find our keys on. 

``` bash
$ gpg --list-secret-keys --with-subkey-fingerprint

[keyboxd]
---------
sec#  ed25519 YYYY-MM-DD [C]
      74BD90B590F9AA984391707383308CA85B65951C
uid           [ultimate] Bryan Dodd <bryan@dodd.dev>
ssb>  ed25519 YYYY-MM-DD [S]
      ................................5400D8FF
      Card serial no. = 0006 12345678       ## <==
ssb>  cv25519 YYYY-MM-DD [E]
      ................................B4C98A69
      Card serial no. = 0006 12345678       ## <==
ssb>  ed25519 YYYY-MM-DD [A]
      ................................EB5285E8
      Card serial no. = 0006 12345678       ## <==
```

When you change to a different Yubikey on your machine, you'll need to alert GnuPG that your keys need to be remapped to the currently inserted key:

``` bash
$ gpg-connect-agent "scd serialno" "learn --force" /bye

S SERIALNO D1234567891234567891234567891234
OK
S PROGRESS learncard k 0 0
S PROGRESS learncard k 0 0
S PROGRESS learncard k 0 0
OK
```
