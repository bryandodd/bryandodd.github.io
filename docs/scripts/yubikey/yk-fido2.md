---
title: Yubikey FIDO2/Passkey Configuration
---

# Yubikey FIDO2 / Passkey Configuration

FIDO U2F and FIDO2 are both authentication protocols developed by the FIDO Alliance to enhance security and eliminate weak passwords. The main difference between them is that U2F is a second-factor protocol only, while FIDO2 can be used for passwordless (single-factor) or second-factor. 

U2F is still useful in the sense that it is phishing-resistant, but FIDO2 has quickly become more prominent with the rise of Passkeys.


## What They Are

Passkeys use public-key cryptography, which sounds intimidating but doesn't need to be:

1. When you create a passkey for a website, your device generates two keys:
    * A private key, which stays securely stored on your device.
    * A public key, which gets registered with the website.

2. When you try to log in:
    * The website checks your public key.
    * Your device confirms your identity (i.e., fingerprint, Face ID, or PIN).
    * If everything matches, you're logged in -- no password required.

## Resident vs Non-Resident

FIDO2 uses a concept of discoverable (resident) and non-discoverable (non-resident) credentials. The primary difference between them is also the functional determiner as to whether or not it supports single-factor authentication: storage of the credential on-device. 

Resident credentials require storage space - non-resident credentials are computationally determined.

| Feature | Resident<br />(Discoverable) | Non-Resident<br />(Non-Discoverable) |
| ------- | --------------- | --------------- | 
| Stored on Authenticator | Yes | No |
| Username Required | No | Yes | 
| Works for Passwordless | Yes | No (requires username first) | 
| Supported by Passkeys | Yes | No |
| Storage Limitation | Yes (limited slots on hardware keys) | No (server stores user records)


## FIDO2 (WebAuthn + CTAP) vs Passkeys

FIDO2 predates Passkeys, but the two are very closely related. They both utilize the same underlying technology -- WebAuthn (web authentication standard), and CTAP (a protocol for talking to security keys) -- but operate in different ways. 

Passkeys can be thought of more as an extension of FIDO2. Where FIDO2 was intended to be device-bound, Passkeys were intended to have multi-device support and synchronize across cloud services.

| Feature | FIDO2<br />(Traditional) | Passkeys |
| ------- | ------------------------ | -------- |
| Storage | Tied to a hardware device (like Yubikey) | Synced across devices via cloud<br />(Apple, Google, Microsoft) |
| Portability | Needs the same hardware key each time | Works across devices via cloud sync |
| Backup & Recovery | If you lose the key, you need a backup key | Cloud-based, so you can recover from another device |
| Usability | Requires inserting and tapping a Yubikey or using built-in auth | Works across devices without needing a separate security key |
| Adoption | Mostly used in enterprise / high-security setups | Designed for mainstream users to replace passwords easily |


## Bottom Line

The definition of *Passkey* versus *FIDO2* has been loosely defined, and most websites or services don't actually differentiate between the two - even if their service supports one over the other. 

Most websites that state they support ***Passkeys*** actually support ***FIDO2*** -- and because Passkeys are an extension of FIDO2, both are supported. On these sites, you can register one or more Yubikeys for authentication, or create a passkey via your mobile device or password manager. 

Other websites claim to support "***passwordless authentication***," but in reality only support ***Passkeys***. FIDO2 can be "passwordless" ... but if a site only supports a *mobile-first* implemenation (iCloud, Google, password managers, etc.), what the user experiences is the ability to enable passwordless only from an iPhone or Android device. 

Further still, some sites that claim to support Passkeys actually only support them as a second-factor. This can get even more muddied when they only support using them as a second factor on mobile devices. In practice, these sites only support FIDO U2F -- which could easily be opened up to significantly more device types. They are being artificially limited by a lack of understanding about which technology they're actually using and supporting.  


## Yubikeys

Yubikeys do not support Passkeys in the strictest definition of the technology -- however, because most sites that support Passkeys actually support FIDO2, Yubikeys can be used quite easily. 

***Key Takeaway:*** If you're going to use Yubikey for FIDO2 authentication:

1. Purchase multiple devices and configure them at the same time.
2. Keep a log of which sites and services you've registered your device with. Your password manager makes an excellent location to store this information. 
3. Note which apps / services are RESIDENT vs NON-RESIDENT.

### Preparing Yubikey for Passkeys

If you choose to use Yubikey to store your Passkeys, you need to setup your FIDO2 PIN ahead of time. Unlike with PIV and OpenPGP where Yubikey comes with default PINs set from the factory, FIDO2 does not.

``` bash
ykman fido access change-pin -n XXXXXXXXXX
```

While we're here, we should touch on "user presence" (UP) vs "user verification" (UV). The concept is very simple -- presence requires that you *touch* the Yubikey, and verification will authenticate you somehow (i.e., password, PIN, fingerprint, Face ID, etc.). If you want to enable User Verification, you can do that with the following command (substituting your PIN for the X's):

``` bash
ykman fido config toggle-always-uv -P XXXXXXXXXX
```

And with that, your Yubikey is ready to register Passkeys. 


## Website Support

Passkeys are still relatively new, and not everyone supports it yet.  But there are a few resources you can check out to identify sites you regularly use that *do* support Passkeys.

* [Passkeys.directory](https://passkeys.directory/) is a community-driven index of websites, apps, and services that offer signing up with passkeys.
* [Passkeys.io](https://www.passkeys.io/who-supports-passkeys) maintains a list of sites and apps that have implemented Passkeys as a full password replacement - meaning the passkey option is visibly prominent on the login screen. Services that require you to enter a username before being prompted (or that use WebAuthn as a second-factor method) are not listed.
* [Passkeys.com](https://www.passkeys.com/websites-with-passkey-support-sites-directory) is another list of Passkey-supported sites.
* [Keeper Security](https://www.keepersecurity.com/passkeys-directory/) also maintains a list of websites that support Passkey authentication.


## Keep Track of Credentials

Remember to keep a record of which sites and services you've registered your Yubikeys with, and whether or not that site has a `resident` or `non-resident` credential. Because resident credentials are discoverable (stored on Yubikey itself), you can retrieve a list of those credentials at any time:

``` bash
ykman fido credentials list --pin 654321

Credential ID  RP ID                    Username            Display name      
ba8f5853...    login.microsoft.com      jdoe@example.com    John Doe        
b5ce1806...    adobe.com                jdoe@example.com    jdoe@example.com  
04692f9f...    amazon.com               john.d@gmail.com    John D        
a771def9...    github.com               johndoe             John Doe        
ac8bb687...    google.com               myother@email.com   John Doe        
0e4a9165...    www.linkedin.com         jdoe@example.com    jdoe@example.com
5c077083...    account.microcenter.com  jdoe@example.com    jdoe@example.com
4278543d...    uber.com                 jdoe@example.com    jdoe@example.com
```

Non-resident credentials can only be identified from your memory. If you know you registered a key with a particular website or service but the credential is not listed when you run the "list" command, that credential is ***non-resident***. 
