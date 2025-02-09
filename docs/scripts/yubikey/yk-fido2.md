---
title: Yubikey FIDO2/Passkey Configuration
---

# Yubikey FIDO2 / Passkey Configuration

Passkeys are gaining steam as a new method of authenticating to websites and apps without using passwords. If you've seen any posts or videos about "passwordless" logon, Passkeys are one method commonly referred to in this context. 

Instead of using a username and password, you authenticate with your fingerprint, Face ID, or a device PIN. Passkeys are considered more secure and easier to use because they can't be guessed or phished the way traditional passwords can. 


## What They Are

Passkeys use public-key cryptography, which sounds intimidating but doesn't need to be:

1. When you create a passkey for a website, your device generates two keys:
    * A private key, which stays securely stored on your device.
    * A public key, which gets registered with the website.

2. When you try to log in:
    * The website checks your public key.
    * Your device confirms your identity (i.e., fingerprint, Face ID, or PIN).
    * If everything matches, you're logged in -- no password required.

***Why they're better:***

* They're more secure. No passwords mean hackers can't steal or guess them.
* They're phish-proof. Passkeys only work for the websites they were created for, so scammers can't trick you.
* They're easy to use. You don't have to remember any passwords or fiddle with password managers.
* They work across devices. Your passkeys can sync across multiple devices using iCloud Keychain, Strongbox, and other password managers.

Passkeys come in a couple of flavors: multi-device or single-device. Your phone and your password manager are two great examples of multi-device Passkeys. They're syncd across your devices and make accessing the sites and apps you've registered them for highly convenient. If you require additional security, however, a single-device Passkey may be the right choice. 

They work exactly like they sound. Keys are provisioned individually and locked to the device you created them with - like Yubikeys. It offers a higher level of security, but this is where having multiple keys at setup-time is helpful. You can provision your keys all at once, then stash one Yubikey somewhere safe as a backup. 


### FIDO-U2F vs FIDO2

FIDO-U2F (universal second factor) has been around longer. Its development was a collaboration between FIDO Alliance and Google, and in practice it has the same effect as the 6-digit codes everyone is familiar with using today. It can only be used for 2FA, but it is generally more secure than TOTP because you can't phish FIDO-U2F. 

FIDO2 is a melding of WebAuthn (Web Authentication) and CTAP2 (Client To Authenticator Protocol), developed by FIDO Alliance, W3C, and big-tech. And this is where we get to concepts like *passwordless authentication*. FIDO2 can function as either a first-factor (password replacement) or second-factor (like U2F). When you register a FIDO2 authenticator, the authenticator generates a public-private key pair. The private key is stored on the device, while the public key is registered by the site or service you created it with. When you login, you need only a fingerprint, face scan, PIN code, or the device itself instead of a password. 


### Why They're Frustrating

Passkeys leverage FIDO2 and WebAuthn to create either *discoverable* or *non-discoverable* credentials. Passkeys are primarily the 'discoverable' variety. This means that you shouldn't need to enter an identifier on a website to login, because the site or app should be able to see your Yubikey and locate the credentials stored there that are associated with them. But in practice, it usually doesn't work that way. In most implementations, you still have to provide your username or email address and then select an option to inform the app or site that you want to authenticate with your Passkey instead of your password.

The second frustrating aspect of Passkeys is that they're non-exportable. And yes, I understand that was the whole point behind them in the first place ***and*** a key aspect of their security. But what this means, then, is that even if you're trying to be responsible by purchasing multiple Yubikeys to set one or two aside as spares or "emergency keys", unless you registered each key one after the other when you setup your Passkeys, you're probably going to end up with a "mishmash" of Passkeys on different Yubikeys. 

***TL;DR*** - Passkeys are great for security, but throttle your enthusiasm for them because ensuring you've got backups for all of your important sites or apps is likely going to be a frustrating experience. It's best to spend time loading Passkeys on the front end, before keys end up separated (keychain, safe, off-site location, etc.).


!!! angryface "Portable-Only (Keychain) Passkeys"

    Be aware that a number of sites that claim to support Passkeys only really support the Apple implementation. Others support only "mobile" Passkeys. This simply means that unless you're creating the Passkey within your iCloud Keychain, Android, password manager, or (in some cases) Google Chrome, that site isn't going to let you create one. 

    Examples of sites or apps with this restriction include [Robinhood](https://robinhood.com/), [Ring.com](https://ring.com/), [Nintendo](https://www.nintendo.com), and [Walmart](https://www.walmart.com/). If the site or app claims to support Passkeys, but only from within their mobile app, this is an indicator that you won't be able to provision a Passkey using Yubikey. 


!!! angryface "Passkeys That Aren't Passkeys"

    Just because a website claims to support Passkeys doesn't mean it does. Passkeys enable single-factor authentication. If the credential isn't discoverable (resident), its not a Passkey. And if it's not a Passkey, it's MFA-only.


!!! angryface "Passkeys That Don't *'Passkey'*"

    What's especially frustrating are sites that claim to support Passkeys, have administrative functions to let you add them, act as though you've successfully registered them, but then never actually implement them.  [eBay](https://www.ebay.com) and [PayPal](https://www.paypal.com) are two excellent examples in this category. 
    
    I spent over an hour on the phone with eBay support attempting to submit a ticket because the site allowed me to create Passkeys with my Yubikeys, but the only place it actually prompts for or allows Passkeys is when navigating to the specific part of the user settings page to manage your Passkeys.  


## Preparing Yubikey for Passkeys

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


## View FIDO2 / Passkeys

You'll only be able to retrieve the list of *resident* (discoverable) FIDO2 credentials saved to Yubikey, but you can do that with the following command:

``` bash
$ ykman fido credentials list --pin 654321

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

