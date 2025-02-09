---
title: PGP Keys
icon: simple/gnuprivacyguard
---

# PGP Keys

## General Information

<div class="grid cards" markdown>

-   ***Key Markings:***

    | Mark | Description |
    | ---- | ----------- |
    | `sec` | SECret key |
    | `ssb` | Secret SuBkey |
    | `pub` | PUBlic key |
    | `sub` | public SUBkey |

-   ***Key Flags:*** (1)
    { .annotate }

    1.  [RFC4880-5.2.3.21](https://datatracker.ietf.org/doc/html/rfc4880#section-5.2.3.21)
    
    | Flag | Character | Description |
    | ---- | --------- | ----------- |
    | `0x01` | `C` | Key Certification |
    | `0x02` | `S` | Sign Data |
    | `0x04` | `E` | Encrypt Communications |
    | `0x08` | `E` | Encrypt Storage |
    | `0x10` | ` ` | Split Key |
    | `0x20` | `A` | Authentication |
    | `0x80` | ` ` | Shared key |

    `0x04` + `0x08` = `0xC` (encrypt both communications & storage)

</div>

See also, [Signature Classes (RFC4880-5.2)](https://datatracker.ietf.org/doc/html/rfc4880#section-5.2)

## Anatomy

``` bash
$ gpg --list-secret-keys --with-subkey-fingerprint

[keyboxd]
---------
sec#  ed25519 YYYY-MM-DD [C]
      7777777777777777777777777777777777777777
uid           [ultimate] Joe Schmoe <joe@schmoe.com>
ssb>  ed25519 YYYY-MM-DD [S]
      6666666666666666666666666666666666666666
      Card serial no. = 0006 12345678
ssb>  cv25519 YYYY-MM-DD [E]
      5555555555555555555555555555555555555555
      Card serial no. = 0006 12345678
ssb>  ed25519 YYYY-MM-DD [A]
      4444444444444444444444444444444444444444
      Card serial no. = 0006 12345678
```

PGP keys are most commonly identified via their `fingerprint`. These fingerprints may or may not include spaces - the following examples are the most common ways keys are written:
* `74BD90B590F9AA984391707383308CA85B65951C`
* `74BD 90B5 90F9 AA98 4391 7073 8330 8CA8 5B65 951C`

Additionally, keys may be referred to by their "id" _(or "key id")_. A Key ID consists of the last four chunks from it's signature. Using the example above, the Key ID is:  `83308CA85B65951C`

```
Fingerprint:  74BD 90B5 90F9 AA98 4391 7073 8330 8CA8 5B65 951C
     Key ID:                                ^^^^ ^^^^ ^^^^ ^^^^
```

`uid` | User ID

:   Identifies the user and level of trust for the user ID.

    - `[ultimate]` : Fully trusted (usually your own keys)
    - `[full]` : Fully trusted by you
    - `[marginal]` : Partially trusted
    - `[unknown]` : No explicit trust level assigned
    - `[expired]` : Key has expired
    - `[revoked]` : Key has been revoked
    - `[disabled]` : Key has been disabled

`sec#`

:   Indicates the secret key material is not available (ex: key is stored on a smartcard or external hardware)

`ssb>`

:   Indicates the secret key material is stored externally (as above), but accessible. Keys have been ***stubbed***.


## Examining Key Files

``` bash
$ gpg --show-keys -vvv --with-subkey-fingerprint pub.key

$ gpg --show-keys -vvv --with-subkey-fingerprint sub.key
```

## Importing Keys

!!! warning

    Importing private subkeys is discouraged. Store subkeys to external devices (like Yubikey) and only stub your private subkeys locally. 

```bash
$ gpg --import pub.key
```

### From a Keyserver

``` bash
$ gpg --keyserver hkps://keys.openpgp.org --recv-keys 0x83308CA85B65951C

# If you've not already imported a key:
gpg: key 83308CA85B65951C: public key "Bryan Dodd <bryan@dodd.dev>" imported
gpg: Total number processed: 1
gpg:               imported: 1

# If you've already previously imported a key, you'll see:
gpg: key 83308CA85B65951C: "Bryan Dodd <bryan@dodd.dev>" not changed
gpg: Total number processed: 1
gpg:              unchanged: 1
```

### Direct Download

Using a known public key URL, individual keys can be imported manually.
``` bash
$ curl https://path/to/the/key/bryandodd.asc | gpg --import

# New key:
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  1367  100  1367    0     0   8603      0 --:--:-- --:--:-- --:--:--  8762
gpg: key 83308CA85B65951C: public key "Bryan Dodd <bryan@dodd.dev>" imported
gpg: Total number processed: 1
gpg:               imported: 1

# Already exists:
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  1367  100  1367    0     0   8603      0 --:--:-- --:--:-- --:--:--  8762
gpg: key 83308CA85B65951C: "Bryan Dodd <bryan@dodd.dev>" not changed
gpg: Total number processed: 1
gpg:              unchanged: 1
```

### Trusting Keys

```bash
gpg --edit-key 7777777777777777

gpg (GnuPG) 2.4.3; Copyright (C) 2023 g10 Code GmbH
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Secret subkeys are available.

pub  ed25519/7777777777777777
     created: YYYY-MM-DD  expires: never       usage: C   
     trust: unknown       validity: unknown
ssb  ed25519/6666666666666666
     created: YYYY-MM-DD  expires: never       usage: S   
     card-no: 0006 12345678
ssb  cv25519/5555555555555555
     created: YYYY-MM-DD  expires: never       usage: E   
     card-no: 0006 12345678
ssb  ed25519/4444444444444444
     created: YYYY-MM-DD  expires: never       usage: A   
     card-no: 0006 12345678
[ unknown] (1). Joe Schmoe <joe@schmoe.com>

gpg> trust

pub  ed25519/7777777777777777
     created: YYYY-MM-DD  expires: never       usage: C   
     trust: unknown       validity: unknown
ssb  ed25519/6666666666666666
     created: YYYY-MM-DD  expires: never       usage: S   
     card-no: 0006 12345678
ssb  cv25519/5555555555555555
     created: YYYY-MM-DD  expires: never       usage: E   
     card-no: 0006 12345678
ssb  ed25519/4444444444444444
     created: YYYY-MM-DD  expires: never       usage: A   
     card-no: 0006 12345678
[ unknown] (1). Joe Schmoe <joe@schmoe.com>

Please decide how far you trust this user to correctly verify other users' keys
(by looking at passports, checking fingerprints from different sources, etc.)

  1 = I don't know or won't say
  2 = I do NOT trust
  3 = I trust marginally
  4 = I trust fully
  5 = I trust ultimately
  m = back to the main menu

Your decision? 5
Do you really want to set this key to ultimate trust? (y/N) y
```

## Key Stubs

If your keys were previously saved to Yubikey, you don't want to move copies of your keys onto each computer you use - that would defeat the purpose of having your keys on a secure device like Yubikey. Instead, you want to "stub out" your keys so that the machine you're using refers back to your Yubikey for all private key operations. 

Stubs point to specific keys for specific Yubikey serial numbers. This means GPG will always refer back to the last Yubikey you were using. 

In order to use multiple Yubikeys with the same key loaded, you must force GPG to re-scan your currently connected Yubikey to re-map your stubs. 

``` bash
$ gpg-connect-agent "scd serialno" "learn --force" /bye

S SERIALNO D1234561234561234561234561234561
OK
S PROGRESS learncard k 0 0
S PROGRESS learncard k 0 0
S PROGRESS learncard k 0 0
OK
```

You can verify keys are stubbed by listing keys and looking for the `>` reference after the key identifier:

``` bash
$ gpg --list-secret-keys

[keyboxd]
---------
sec#  ed25519 YYYY-MM-DD [C]    # certifying key is not loaded on the machine (or yubikey)
      7777777777777777777777777777777777777777
uid           [ultimate] Joe Schmoe <joe@schmoe.com>
ssb>  ed25519 YYYY-MM-DD [S]    # successfully stubbed
ssb>  cv25519 YYYY-MM-DD [E]    # successfully stubbed
ssb>  ed25519 YYYY-MM-DD [A]    # successfully stubbed
```

## Yubikey Info

To retrieve details for the currently loaded Yubikey:

``` bash
$ gpg --card-status -vvv

gpg: using character set 'utf-8'
gpg: enabled compatibility flags:
gpg: using subkey 6666666666666666 instead of primary key 7777777777777777
Reader ...........: Yubico YubiKey OTP FIDO CCID
Application ID ...: D1234561234561234561234561234561
Application type .: OpenPGP
Version ..........: 3.4
Manufacturer .....: Yubico
Serial number ....: 12345678
Name of cardholder: Joe Schmoe
Language prefs ...: en
Salutation .......: 
URL of public key : [not set]
Login data .......: joe@schmoe.com
Signature PIN ....: not forced
Key attributes ...: ed25519 cv25519 ed25519
Max. PIN lengths .: 127 127 127
PIN retry counter : 6 0 6
Signature counter : 29
KDF setting ......: off
UIF setting ......: Sign=off Decrypt=off Auth=off
Signature key ....: 6666 6666 6666 6666 6666  6666 6666 6666 6666 6666
      created ....: 2020-01-28 16:32:56
Encryption key....: 5555 5555 5555 5555 5555  5555 5555 5555 5555 5555
      created ....: 2020-01-28 16:36:40
Authentication key: 4444 4444 4444 4444 4444  4444 4444 4444 4444 4444
      created ....: 2020-01-28 16:38:00
General key info..: sub  ed25519/6666666666666666 2020-01-28 Joe Schmoe <joe@schmoe.com>
sec#  ed25519/7777777777777777  created: 2020-01-28  expires: never     
ssb>  ed25519/6666666666666666  created: 2020-01-28  expires: never     
                                card-no: 0006 12345678
ssb>  cv25519/5555555555555555  created: 2020-01-28  expires: never     
                                card-no: 0006 12345678
ssb>  ed25519/4444444444444444  created: 2020-01-28  expires: never     
                                card-no: 0006 12345678
```

To quickly identify where subkey pointers lead:

``` bash
$ gpg --list-secret-keys --with-subkey-fingerprint -vvv

gpg: using character set 'utf-8'
gpg: enabled compatibility flags:
gpg: using pgp trust model
gpg: key 7777777777777777: accepted as trusted key
[keyboxd]
---------
sec#  ed25519 2020-01-28 [C]
      7777777777777777777777777777777777777777
uid           [ultimate] Joe Schmoe <joe@schmoe.com>
ssb>  ed25519 2020-01-28 [S]
      6666666666666666666666666666666666666666
      Card serial no. = 0006 12345678
ssb>  cv25519 2020-01-28 [E]
      5555555555555555555555555555555555555555
      Card serial no. = 0006 12345678
ssb>  ed25519 2020-01-28 [A]
      4444444444444444444444444444444444444444
      Card serial no. = 0006 12345678
```