---
title: PGP Primer
icon: octicons/info-16
---

# A quick PGP primer ...

## What even is "PGP" ?

If you've ever been curious about (or needed to know) how to encrypt files for security, PGP (or *Pretty Good Privacy*) is a simple and elegant solution - but can be a little intimidating if you've never used it before. Cryptography is a very deep subject with more rabbit holes than Reddit at 11 PM, and I wouldn't even be able get you all the way there myself. Forunately, there are a plethora of excellent resources available online if you're so inclined. This will only cover the basics, but it will be enough to actually enable you to use it. 

PGP is ***public-key cryptography***. Users generate a unique pair of keys (a <span style="color: var(--md-code-hl-string-color)">**public**</span> key that can be shared openly and freely, and a <span style="color: var(--md-code-hl-special-color)">**private**</span> key that must be kept secret) that can be used for:

* Encrypting messages so only the intended recipient can read them
* Encrypting files to backup sensitive data or share over the internet without risking exposure
* Signing messages or files to prove that they came from you and haven't been tampered with
* Sign documents and contracts to ensure authenticity

## Identifying Keys

=== "Public Keys"

    Your <span style="color: var(--md-code-hl-string-color)">**public key**</span> can be thought of as an open mailbox. Anyone can drop a message or package into it, and you share it with people so they can send things to you securely. These keys usually have one of the following file extensions:

    | Extension | Description |
    | --------- | ----------- |
    | `.pub` | Commonly used to designate public keys, especially with command-line tools like GPG and OpenSSH. |
    | `.asc` | Public keys saved in ASCII-armored format (this just means that it can easily be rendered in plain text) for easy sharing. |
    | `.key` | A generic file extension that you'll sometimes seen used for public keys. |
    | `.pgp` or `.gpg` | Public keys saved in binary format - meaning they're meant to be read by computers and not people. |

=== "Private Keys"

    Your <span style="color: var(--md-code-hl-special-color)">**private key**</span> is the only key for that mailbox. It's required to retrieve the messages and packages people drop in your mailbox, and must be treated carefully -- you wouldn't want to save this key to a USB and leave it laying around, or save the key to a computer you share with other people. These keys usually have one of the following file extensions:

    | Extension | Description |
    | --------- | ----------- |
    | `none` | It's not uncommon to see private key files with no file extension at all to emphasize their security and discourage accidental sharing. |
    | `.asc` | Private keys saved in ASCII-armored format. |
    | `.key` | A generic file extension for private keys, but it is rare to see private PGP keys with this extension. |
    | `.pgp` or `.gpg` | Private keys saved in binary format. |

Because file naming can be inconsisent, I try to following one of two naming convensions to help keep them sorted:

<div class="grid cards" markdown>

-   ***Convention 1***

    * `keyname.pub` for the public key, and
    * `keyname` for the private key.

-   ***Convention 2***

    * `keyname_pub.asc` for the public key, and
    * `keyname_private.asc` for the private key.

</div>

!!! question "Key confusion?"

    If you get confused about which key is which, the keys themselves can be opened in any text editor (provided you saved your keys as ASCII-armored). The first and last lines of each file will identify the type of key you have: `-----BEGIN PGP PUBLIC KEY BLOCK-----` vs `-----BEGIN PGP PRIVATE KEY BLOCK-----`


## Encryption and Signatures

The two most common uses for PGP keys are encrypting files or signing them to verify authenticity and that the file hasn't been tampered with. Sometimes you'll see both used together: encryption + signature. 

Files are encrypted using <span style="color: var(--md-code-hl-string-color)">**public**</span> keys, and decrypted with <span style="color: var(--md-code-hl-special-color)">**private**</span> keys. When signing files, the opposite is true: Sign files using your <span style="color: var(--md-code-hl-special-color)">**private**</span> key, and your recipient can verify the file came from you by validating your signature with your <span style="color: var(--md-code-hl-string-color)">**public**</span> key. 

| Use-Case | Public Key | Private Key |
| -------- | :--------: | :---------: |
| Files / Messages | **`encrypt`** | **`decrypt`** |
| Signing | **`validate`** | **`sign`** |

=== "Encrypting Files"

    Lets look at an example of encrypting files or messages:

    Say that you have a file named `secret-message.txt` that you want to encrypt and send to me. Because you have my <span style="color: var(--md-code-hl-string-color)">**public**</span> PGP key, you can encrypt the file so that only I can decrypt it. The new encrypted version of the file will have one of two possible filenames[^1] based on ***how*** you encrypted it:

    * If you encrypted it in ASCII-armored format, the filename would be `secret-message.txt.asc`. 
    * If you encrypted it in binary format, the filename would be `secret-message.txt.pgp`[^2].

=== "Signing Files"

    This time, when you encrypt the file named `secret-message.txt`, instead of just simply encrypting the file, you select the option to "sign and encrypt" it. When this option is used, you are using ***my*** <span style="color: var(--md-code-hl-string-color)">**public**</span> key to encrypt the file, and ***your*** <span style="color: var(--md-code-hl-special-color)">**private**</span> key to sign it. 

    The resulting file will still be named `secret-message.txt.asc`, but the filesize will be a tiny bit larger because your signature is attached. 

    When I receive the file and decrypt it with ***my*** <span style="color: var(--md-code-hl-special-color)">**private**</span> key, I will use ***your*** <span style="color: var(--md-code-hl-string-color)">**public**</span> key to verify it was you that signed it and confirm that no one has tampered with the file after you sent it. 

=== "Encrypt and Sign Separately"

    Occasionally you may have a need to encrypt a file and sign it separately. This results in two files - your encrypted file, and a signature (`.sig`) file. 

    This is called a "detached signature," and you'll more commonly see it when the encrypted file is in binary format. For example, lets say that you wanted to send me a photograph, `winter-in-maine.jpg`.

    When you encrypt the file (using my <span style="color: var(--md-code-hl-string-color)">**public**</span> key), the resulting output is a new encrypted file named `winter-in-main.jpg.pgp`. When you sign it, a new file named `winter-in-maine.jpg.sig` is created. You'll need to send both files to me, but once I receive them the process is the same as before.

    I'll use my <span style="color: var(--md-code-hl-special-color)">**private**</span> key to decrypt the `.pgp` file to get the original photo, and I'll verify your signature separately using your <span style="color: var(--md-code-hl-string-color)">**public**</span> key.


## Creating Your Keys

Before you can encrypt anything, you need a keypair - that's your private and public PGP keys. 

:   :material-microsoft-windows: Windows: You'll need to download and install [gpg4win](https://www.gpg4win.org/download.html).

:   :material-apple: Apple: `brew install gnupg`

:   :material-debian: Debian/Ubuntu: `sudo apt install gnupg`

For the rest of this document, we'll focus on command-line tooling on macOS/Linux. To verify you've got the tools installed, run `gpg --list-keys`. If you see output similar to the example below, you're in good shape. If you're just installing and using gnupg for the first time, you won't have any keys on your system, but running this command will create your keyring (where your keys will ultimately reside).

``` bash
$ gpg --list-keys

gpg: directory '/root/.gnupg' created
gpg: keybox '/root/.gnupg/pubring.kbx' created
gpg: /root/.gnupg/trustdb.gpg: trustdb created
```

### Generating Your Keys

In the next step, we'll actually create a set of keys. Reference the notes within the code snippet below:

``` bash
$ gpg --full-generate-key --expert

gpg (GnuPG) 2.2.27; Copyright (C) 2021 Free Software Foundation, Inc.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Please select what kind of key you want:
   (1) RSA and RSA (default)
   (2) DSA and Elgamal
   (3) DSA (sign only)
   (4) RSA (sign only)
   (7) DSA (set your own capabilities)
   (8) RSA (set your own capabilities) # (2)
   (9) ECC and ECC
  (10) ECC (sign only)
  (11) ECC (set your own capabilities) # (8)
  (13) Existing key
  (14) Existing key from card
Your selection? 8 # (1)

Possible actions for a RSA key: Sign Certify Encrypt Authenticate 
Current allowed actions: Sign Certify Encrypt  # <<== NOTE THE CURRENT ACTIONS

   (S) Toggle the sign capability
   (E) Toggle the encrypt capability
   (A) Toggle the authenticate capability
   (Q) Finished

Your selection? S # (3)

Possible actions for a RSA key: Sign Certify Encrypt Authenticate 
Current allowed actions: Certify Encrypt 

   (S) Toggle the sign capability
   (E) Toggle the encrypt capability
   (A) Toggle the authenticate capability
   (Q) Finished

Your selection? E # (4)

Possible actions for a RSA key: Sign Certify Encrypt Authenticate 
Current allowed actions: Certify 

   (S) Toggle the sign capability
   (E) Toggle the encrypt capability
   (A) Toggle the authenticate capability
   (Q) Finished

Your selection? Q
RSA keys may be between 1024 and 4096 bits long.
What keysize do you want? (3072) 4096 # (5)
Requested keysize is 4096 bits
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0) 0 # (6)
Key does not expire at all
Is this correct? (y/N) y

GnuPG needs to construct a user ID to identify your key.

Real name: John Smith # (7)
Email address: john@smith.com
Comment: 
You selected this USER-ID:
    "John Smith <john@smith.com>"

Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? O
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
gpg: key 56DBD75D1A3FA918 marked as ultimately trusted
gpg: directory '/root/.gnupg/openpgp-revocs.d' created
gpg: revocation certificate stored as '/root/.gnupg/openpgp-revocs.d/1C2EB5D40EBAC3871641B2E756DBD75D1A3FA918.rev'
public and secret key created and signed.

pub   rsa4096 2025-02-01 [C] # (9)
      1C2EB5D40EBAC3871641B2E756DBD75D1A3FA918
uid                      John Smith <john@smith.com>
```

1.  We're creating an RSA key because it's the most universally accepted, but another type I would recommend is ECC (option 11).
2.  Selecting an option for "set your own capabilities" is important because we want to set our keys up for better security.
3.  We're explicitly disabling the "sign" capabilities for our first key. All we want this key to do is CERTIFY.
4.  Again, we're explicitly disabling the "encrypt" capability so our key is only usable for CERTIFY actions.
5.  When creating an RSA key, it's strongly advised that you use maximum bitlength - `4096` bits.
6.  For simplicity, we're setting this key to never expire. Realistically, this will depend on the level of security you need.
7.  Use your real name and email address. The identity you enter here will be how people are able to find and use your key.
8.  If you had selected ECC, you would next be asked to choose an elliptic curve to use. My suggestion is to use `ECC (1) Curve 25519`.
9.  The last thing output by the generate command is the confirmation that we've generated a CERTIFY only key, along with it's Key ID.

During the process, you'll be prompted to enter a passphrase (password) to protect your keys. ***You should not skip this step.*** 

We specifically created our first key to be our "master certifying" key only. In order to perform operations like Encryption, Signing, and Authenticate, we'll create separate subkeys that are signed by our CERTIFY key. 

If your intention is to use keys to encrypt and protect files or data, it's imperative that you select a good, strong password that you have never used anywhere else. Store your password in a secure password vault (like [Strongbox](https://strongboxsafe.com/) for Apple devices or [:simple-keeper: Keeper](https://www.keepersecurity.com/)).

We'll now proceed with creating our SUBKEYS.  Before we do though, take note of our key ID: `1C2EB5D40EBAC3871641B2E756DBD75D1A3FA918`.

``` bash
$ gpg --expert --edit-key 1C2EB5D40EBAC3871641B2E756DBD75D1A3FA918

gpg (GnuPG) 2.2.27; Copyright (C) 2021 Free Software Foundation, Inc.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Secret key is available.

sec  rsa4096/56DBD75D1A3FA918
     created: 2025-02-01  expires: never       usage: C   
     trust: ultimate      validity: ultimate
[ultimate] (1). John Smith <john@smith.com>

gpg> addkey # (1)
Please select what kind of key you want:
   (3) DSA (sign only)
   (4) RSA (sign only)
   (5) Elgamal (encrypt only)
   (6) RSA (encrypt only)
   (7) DSA (set your own capabilities)
   (8) RSA (set your own capabilities)
  (10) ECC (sign only)
  (11) ECC (set your own capabilities)
  (12) ECC (encrypt only)
  (13) Existing key
  (14) Existing key from card
Your selection? 4 # (2)
RSA keys may be between 1024 and 4096 bits long.
What keysize do you want? (3072) 4096
Requested keysize is 4096 bits
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0) 0
Key does not expire at all
Is this correct? (y/N) y
Really create? (y/N) y
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.

sec  rsa4096/56DBD75D1A3FA918
     created: 2025-02-01  expires: never       usage: C   
     trust: ultimate      validity: ultimate
ssb  rsa4096/997B1D51C90AE36E # (3)
     created: 2025-02-01  expires: never       usage: S   
[ultimate] (1). John Smith <john@smith.com>
```

1.  Enter `addkey` at the GPG prompt to create our first subkey.
2.  Here, we're creating our SIGNING key only.
3.  The final output shows that our SIGNING key `[S]` is a subkey (`ssb`).

We will follow this same proceedure two more times to generate subkeys for ENCRYPTION and AUTHENTICATION.

``` bash
gpg> addkey
Please select what kind of key you want:
   (3) DSA (sign only)
   (4) RSA (sign only)
   (5) Elgamal (encrypt only)
   (6) RSA (encrypt only)
   (7) DSA (set your own capabilities)
   (8) RSA (set your own capabilities)
  (10) ECC (sign only)
  (11) ECC (set your own capabilities)
  (12) ECC (encrypt only)
  (13) Existing key
  (14) Existing key from card
Your selection? 6 # (1)
RSA keys may be between 1024 and 4096 bits long.
What keysize do you want? (3072) 4096
Requested keysize is 4096 bits
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0) 0
Key does not expire at all
Is this correct? (y/N) y
Really create? (y/N) y
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.

sec  rsa4096/56DBD75D1A3FA918
     created: 2025-02-01  expires: never       usage: C   
     trust: ultimate      validity: ultimate
ssb  rsa4096/997B1D51C90AE36E
     created: 2025-02-01  expires: never       usage: S   
ssb  rsa4096/53F5916662440AC5 # (2)
     created: 2025-02-01  expires: never       usage: E   
[ultimate] (1). John Smith <john@smith.com>

gpg> addkey
Please select what kind of key you want:
   (3) DSA (sign only)
   (4) RSA (sign only)
   (5) Elgamal (encrypt only)
   (6) RSA (encrypt only)
   (7) DSA (set your own capabilities)
   (8) RSA (set your own capabilities)
  (10) ECC (sign only)
  (11) ECC (set your own capabilities)
  (12) ECC (encrypt only)
  (13) Existing key
  (14) Existing key from card
Your selection? 8 # (3)

Possible actions for a RSA key: Sign Encrypt Authenticate 
Current allowed actions: Sign Encrypt  # <<== NOTE THE CURRENT ACTIONS

   (S) Toggle the sign capability
   (E) Toggle the encrypt capability
   (A) Toggle the authenticate capability
   (Q) Finished

Your selection? A # (4)

Possible actions for a RSA key: Sign Encrypt Authenticate 
Current allowed actions: Sign Encrypt Authenticate 

   (S) Toggle the sign capability
   (E) Toggle the encrypt capability
   (A) Toggle the authenticate capability
   (Q) Finished

Your selection? S # (5)

Possible actions for a RSA key: Sign Encrypt Authenticate 
Current allowed actions: Encrypt Authenticate 

   (S) Toggle the sign capability
   (E) Toggle the encrypt capability
   (A) Toggle the authenticate capability
   (Q) Finished

Your selection? E

Possible actions for a RSA key: Sign Encrypt Authenticate 
Current allowed actions: Authenticate 

   (S) Toggle the sign capability
   (E) Toggle the encrypt capability
   (A) Toggle the authenticate capability
   (Q) Finished

Your selection? Q
RSA keys may be between 1024 and 4096 bits long.
What keysize do you want? (3072) 4096
Requested keysize is 4096 bits
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0) 0
Key does not expire at all
Is this correct? (y/N) y
Really create? (y/N) y
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.

sec  rsa4096/56DBD75D1A3FA918
     created: 2025-02-01  expires: never       usage: C   
     trust: ultimate      validity: ultimate
ssb  rsa4096/997B1D51C90AE36E
     created: 2025-02-01  expires: never       usage: S   
ssb  rsa4096/53F5916662440AC5
     created: 2025-02-01  expires: never       usage: E   
ssb  rsa4096/9C06401751C73176 # (6)
     created: 2025-02-01  expires: never       usage: A   
[ultimate] (1). John Smith <john@smith.com>

gpg> save # (7)
```

1.  Select option 6 to create an ENCRYPTION only RSA key.
2.  The output confirms that we've generated a new subkey for ENCRYPTION only `[E]`.
3.  Notice there is no AUTHENTICATION option, so we're going to use option 8 again to specify our own capabilities.
4.  SIGN and ENCRYPT are enabled by default, so we're going to first enable AUTHENTICATION.
5.  Then disable SIGN and ENCRYPT.
6.  Our output confirms we've generated an AUTHENTICATION subkey.
7.  Don't forget to enter the SAVE command. Your keys will not be committed to your keyring until you enter this command.


We can now test to confirm the presence of our public and private keys:

``` bash
$ gpg --list-keys  # Public Keys

/root/.gnupg/pubring.kbx
------------------------
pub   rsa4096 2025-02-01 [C]
      1C2EB5D40EBAC3871641B2E756DBD75D1A3FA918
uid           [ultimate] John Smith <john@smith.com>
sub   rsa4096 2025-02-01 [S]
sub   rsa4096 2025-02-01 [E]
sub   rsa4096 2025-02-01 [A]


$ gpg --list-secret-keys  # Private Keys

/root/.gnupg/pubring.kbx
------------------------
sec   rsa4096 2025-02-01 [C]    # our master CERTIFY key (sec = secret)
      1C2EB5D40EBAC3871641B2E756DBD75D1A3FA918
uid           [ultimate] John Smith <john@smith.com>
ssb   rsa4096 2025-02-01 [S]    # SIGN subkey
ssb   rsa4096 2025-02-01 [E]    # ENCRYPT subkey
ssb   rsa4096 2025-02-01 [A]    # AUTHENTICATE subkey
```

Your new keys are ready to use. 

Where you see `[ultimate]` beside your name and email has to do with something called the "*web of trust*." These keys were generated just now, by you, on your machine, so they're considered to have "ultimate" trust (you *do* trust yourself, after all).

And don't forget that the 40-character long string is your key's "fingerprint."


### Exporting Your Public Key

Your keys are tucked away into your keyring on your machine. You can leave them there, but your public key won't do you much good if no one else has it. 

We'll start by demonstrating how to export your keys is general:

``` bash title="exporting your public key"
$ gpg --output my-public-key.asc --armor --export 1C2EB5D40EBAC3871641B2E756DBD75D1A3FA918
# or you could also use ...
$ gpg --output my-public-key.asc --armor --export john@smith.com
```

The resulting file:

``` asc title="my-public-key.asc"
-----BEGIN PGP PUBLIC KEY BLOCK-----

mQINBGeEHUoBEACccBnDV+TqZ2aPgKzn9wbeM9wU88yqb7PDUCAn+cogDxBJ+AYl
WMz1QOwNBaV9XmhYi4viqwFx6/uKxjskdk8QXe5e2M4+PRuEetY0NgO+4rziWhQL
 ... truncated ... 
j1JvE8wYgPtgiD2LMC0jeuxu3suWBryxedMxZQNAsrb2T5TiecYGvFXmknP0LhfD
WcNh8jj8HCvAJng1f2XXYdkMuOQDQQbl+kfkMP5DNncc/cxUcKsMVfSBA7D82wia
T/0XQEh9wM5/cZ0OcVUgkn+QZRJ588dbg/e7qIqvnE5qmvAiFPyH
=TB7U
-----END PGP PUBLIC KEY BLOCK-----
```

But what if we didn't want to ASCII-armor our key ...

``` bash
$ gpg --output my-public-key-binary.pgp --export john@smith.com
```

You'll get a new file (`my-public-key-binary.pgp`) as requested, but you won't be able to read it with a text editor. If you try, you'll get an ASCII-representation of the key, but much of the data can't be represented in displayable characters on-screen... so trying to open a binary key like this is pointless.

```
?}^hX???q????;$vO]?^??>=z?46???ZX??@?
                                Z??2q???Q(??Ġ??A?ڳfط??!?cnʢ?~??F;
                                                                 {?w?ra?zF)N???f(?P???3R??&}ͷU???????#dC?}???Ql?l?쁸??????εq??ll?0?#?ds}?5?p??\t???<??i??4??F?*?v?n??(?.'?????_!?m?^???k^?'?$x?87?i??FpM?9l??	h?8????0??:Ԓ
                    ???ـuE?Y?|g??cD?{??,???,+?N???x?!??'??`rmn??!h?r7g?Z?d]??wI?{????Eۮ?Zl????'j?݋?(-??$%0?J?x???6?t???զ?.??	G&G)?
?z???.?hL?+oP?\2H?$?)Q??~v???mx?v???%?V?W??L?7????`q????I??֗??)?Uo??ohn Smith <john@smith.com>?N
8!?Z?8???qt? ????0?g?J
                      	
	
       ?
	???0??L??}????&?!????j?2?JyFj?T)???x??܀%!ߪ>y???uS
??ϓv?kF???
```

### Exporting Your Public Key to a Key Server

There used to be a larger number of public key servers available on which you could host your public key, but these days I seem to only have success with one: [https://keys.openpgp.org/](https://keys.openpgp.org/). 

You will need your Key ID instead of the email address for this command:

``` bash
$ gpg --send-key 1C2EB5D40EBAC3871641B2E756DBD75D1A3FA918
```

If you run into an error attempting to send the key via `gpg`, you can always go to the keyserver website and upload your public key manually. 

### Exporting Your Private Keys

!!! danger "Warning!"

    Exporting your private keys make it much easier to lose them, or worse... for someone to steal them. Use good security practices.

``` bash title="Exporting your Master CERTIFY private key"
$ gpg --output master-key.asc --armor --export-secret-key john@smith.com
```

``` bash title="Exporting your private SUBKEYS"
$ gpg --output subkeys.asc --armor --export-secret-subkeys john@smith.com
```

Save these files in a highly secure location. Anyone with access to your Master Certify or Subkeys can perform all operations with your keys.


### Encrypting Files

We're finally ready to encrypt our document: `testdoc.txt`.

``` bash
$ gpg --encrypt --recipient john@smith.com testdoc.txt
```

The result is a new file named `testdoc.txt.gpg` - but this is a binary file that can't be opened in a text editor. 

``` bash
$ gpg --encrypt --armor --recipient john@smith.com testdoc.txt
```

Now we've got a new encrypted file, `testdoc.txt.asc`.

``` asc title="testdoc.txt.asc"
-----BEGIN PGP MESSAGE-----

hQIMA80WR3EvRM3kAQ//UytVbiz/nk3E3nS5tyvgTv4ESIks6XOuMOcFcGM6Y73p
8zcidTuSK0HTPR0xR4O2nZUAWCFa0GY18c41U671xPz2YTULPbNWrImGP7VmRPWP
UpFcs1PpmuneA8fmHkgdhfr9l3rTnb4Efi4eaOTrgHbgLv9lINQHR+xWZ+P56q26
v+xMA37M42eGYn6vnhXThAxchyUTtc/GkZS3xuvP7M3O5P2l1xMA2KzLeR2yh/FZ
9F/kYN0RbhdgzfnKpUSuoJtvYXjrf0jwT6UdZ+Lb9VpLnngMm6PnRXntu85ZL3vZ
fsrCWnMJYU1w+cyXZYTB32qx3ZcTcvR1VuLdaCESH6Wxv7zDRbodtAi814utGeZh
PX7PO0bAZpeIeG4VQG4SxcHgH3BAs+jIc+bGtcvvVYAlLj0ytICKxrWpGz+gZ4aQ
EeSrSVUEmGle92DalsO9jBdJdSt+71VexHyiRodLC8F/zdknSuEFWhfk+S0p5EtC
fAm4RzN+dfzjg4rGZF7eW+2Sax4hwNWEZTAOXOvZQK/XRhV53YYSL7C/LjURxq3l
69cg7Kk7kbopinXhZ7ODjhQdmx9hC4zlLfgCbRiX+Ggvm2vjweSdmAvXPP1GhKXu
zlOPv1bseQjpHIUE6JXtFghGTctHtEwk/rAamw/flAviKNMH6Is4HQ+ElvqqdjfS
fAH0TScdmsDvlcBlqXwSeqn9z5PE3jsKu5NoxBptrPfs/Iikf6TLycBLEZKJLT3d
BHkClVZUpQST3brn67XnATnmWfxApt1MCvfYo5Dbt2I00O+fFuKD7CMdejvj6ZRu
TOy21PkURh/szicowaRMJZ9/B4YlAYJnD9gzt8c=
=8kzx
-----END PGP MESSAGE-----
```

### Signing (and Verifying) Files

We've got our original `testdoc.txt` file, our binary encrypted file (`testdoc.txt.gpg`), and our ascii armored encrypted file (`testdoc.txt.asc`). To sign a file, you don't have to encrypt it first. You can use your keys to sign anything - encrypted or not.

#### Signing an unencrypted file

``` bash
$ gpg --clear-sign --local-user john@smith.com --armor --sign testdoc.txt
```

... produces

``` asc title="testdoc.txt.asc"
-----BEGIN PGP SIGNED MESSAGE-----
Hash: SHA512

This is a simple text file.
We're going to encrypt it.

-----BEGIN PGP SIGNATURE-----

iQJDBAEBCgAtFiEE41qEOKeHhnF02yCS8AWGhDDzo4EFAmeEKhAPHGpvaG5Ac21p
dGguY29tAAoJEPAFhoQw86OBqZsP/ir/2onGOJxxXWOrY9oq8njrOEkFHM7/ppwe
3CFtequ1PfAv2Uic7qC/r7WFtY9DdEd+n+aEPwW09bMkfg4htM2krDGt9zZMeXjI
XE7oihP+oV6zSHZwyv7BwPwN7DPqZk0VhFnjlOkzxjvoduhewDv1uf12iyfr3ilB
Oc3zIASEAEidpUoOhVd0ljvAIv9CF0NixMzn6iLtT3kB6va4Uu3E8ewWlFsgkxfR
HokZVz2STYzNGnMFeN07/ih4N60QjI1uoHiOHR3N8FasHAxRltVGRthgqM//lKMm
3wYZg9MIDUsZuezTBeUVhIAynRN7HBwiB0yd2YToSzM7tcbrYgPgCjQhORrCfD0s
yq0PMb2Id/E/vEga1a4n0gbpZwNFOo8XU1mEhAbaClEapDkqu6GzNMCmsak009B+
OehkI2mZcrBQbDp7gpUE/39kC5AsbpN/iyBckGNoRxHdDy2AVohWnGAPnllG+Eyx
mT3ekltnkHtjXhaiVls1cf+7AU5M1KyYw1dcEa4SGChTIfYGiBZAvy17AXNrR+h8
xAEsKYzCoMlBCbAxKAT96mO0iMx8n7pv+97qQATjYQyYTTv1peggz7QlZcgu81Jo
WFX94o2MNEG58feEPAHxXy013UrqN8HWsHFtKcX2G0y/s6FTM48VrPcgWEX27ydP
BO4ERnPO
=6qwu
-----END PGP SIGNATURE-----
```

The result is a new file that contains the content of the original unencrypted file at the top of the document in a section labeled `PGP SIGNED MESSAGE`, and our signature at the bottom.

We can verify that signature because we have both the public and private keys -- they are *our* keys afterall. 

``` bash
$ gpg --verify testdoc.txt.asc

gpg: Signature made Sun Jan 12 20:46:08 2025 UTC
gpg:                using RSA key 1C2EB5D40EBAC3871641B2E756DBD75D1A3FA918
gpg:                issuer "john@smith.com"
gpg: Good signature from "John Smith <john@smith.com>" [ultimate]
```

#### Signing and encrypting together

If we wanted to both sign *and* encrypt the file simultaneously, we can do that with:

``` bash
$ gpg --encrypt --output testdoc2.txt.asc --sign --armor --recipient john@smith.com testdoc.txt
```

``` asc title="testdoc2.txt.asc"
-----BEGIN PGP MESSAGE-----

hQIMA80WR3EvRM3kAQ//bWdKQNb+kEqyaB6BaiHKnyxb9gD43yzdzVjRfDtlIuEu
uwqScxSrBDhUYSGIplXIklKm3lu3/xK2KxwVX4hPVpK1BeyusW1VQsxULoBjgt05
   ... truncated ...
o6NiMBJADnD0BjfNB3PqPwidveJLO1k3u9nzagZPmAWRhCIzJqI3zpgceYQ0/beg
VC/z8VxZtfZDzXsNLGJoWJ7QRy5sgyNFRRVbOTc4Skl/aJvTST4lnkLF6A1yEH4x
Jf72ORi9+XsXhDdKIb09+iGek31tR5KDJ8ds8XzITjZShiajtyKRWHk=
=jPXp
-----END PGP MESSAGE-----
```

The resulting file contains only a PGP message, but when decrypted, we get the original file content plus the signature:

``` bash
$ gpg --decrypt testdoc2.txt.asc

gpg: encrypted with 4096-bit RSA key, ID 53F5916662440AC5, created 2025-01-12
      "John Smith <john@smith.com>"
This is a simple text file.
We're going to encrypt it.

gpg: Signature made Sun Jan 12 20:51:26 2025 UTC
gpg:                using RSA key 1C2EB5D40EBAC3871641B2E756DBD75D1A3FA918
gpg: Good signature from "John Smith <john@smith.com>" [ultimate]
```

#### Detached Signature

In the earlier example where we signed an unencrypted file, PGP wrapped the content of that file inside of a new file together with the signature. What if we don't want that, though? A "detached signature" is the better option for such a scenario:

``` bash
$ gpg --detach-sign --local-user john@smith.com --armor --sign testdoc.txt
```

By default, this will create a file named `testdoc.txt.asc` - but that can be confusing since we're only signing this file. A better way to do it is to specify the `--output` paramater, and name your file with a `.sig` extension:

``` bash
$ gpg --detach-sign --output testdoc.txt.sig --local-user john@smith.com --armor --sign testdoc.txt
```

!!! info "Important Info"

    The signature file (`.sig` file) and the original document you signed go together. A signature on its own cannot be validated - your signature is only good for the file you originally specified!

The proper way to validate a detached signature is with the following:

``` bash
$ gpg --verify testdoc.txt.sig testdoc.txt
```

However, if you forget to specify the original file, because we followed the naming convention, `gnupg` will try to guess what file we were comparing the signature with:

``` bash
$ gpg --verify testdoc.txt.sig

gpg: assuming signed data in 'testdoc.txt'
gpg: Signature made Sun Jan 12 20:58:18 2025 UTC
gpg:                using RSA key 1C2EB5D40EBAC3871641B2E756DBD75D1A3FA918
gpg:                issuer "john@smith.com"
gpg: Good signature from "John Smith <john@smith.com>" [ultimate]
```

But what if we forgot to transfer both files to our recipient? In this example, we sent the `.sig` file, but forgot to send the actual `testdoc.txt` file:

``` bash
$ gpg --verify testdoc.txt.sig

gpg: no signed data
gpg: can't hash datafile: No data
```

## Wrapping Up

This page only skims the surface of PGP - there's much more to it if you want to use it fully. There are a lot of great resources on PGP, but here are a few to get you started:

* [Web of Trust](https://en.wikipedia.org/wiki/Web_of_trust)
* [Comprehensive Yet Simple Guide for GPG Key/Subkey Encryption, Signing, Verification & Other Common Operations](https://medium.com/code-oil/comprehensive-yet-simple-guide-for-gpg-key-subkey-encryption-signing-verification-other-common-c28fd868cbe7)
* [GnuPG / GPG / OpenPGP](https://github.com/felixd/gnupg)
* [Proper Key Management](https://www.reddit.com/r/GnuPG/comments/vjas2e/proper_key_management/)
* [Sun Knudsen's Privacy Guides](https://github.com/sunknudsen/privacy-guides/tree/master/docs)
    * [How to encrypt, sign, and decrypt messages using GnuPG on macOS](https://www.youtube.com/watch?v=mE8fL5Fu8x8&list=PLBTp6wwfQk3hpkaJKZg_K0QAGLbKi17x4&index=102&t=11s&pp=iAQB)
    * [How to verify PGP digital signatures using GnuPG on macOS](https://www.youtube.com/watch?v=WnNfunEJdQY&list=PLBTp6wwfQk3hpkaJKZg_K0QAGLbKi17x4&index=65&pp=iAQB)
    * [How PGP digital signatures increase privacy and help decentralize the Internet](https://www.youtube.com/watch?v=sC_V5l2Y37k&list=PLBTp6wwfQk3hpkaJKZg_K0QAGLbKi17x4&index=64&pp=iAQB)



[^1]: When I receive your message, I'll see the `.txt` extension in the name and know that the original file you sent me was a text file. You could just as easily have encrypted a Microsoft Word (`.docx`) or Excel (`.xlsx`) file. 

[^2]:
    `.pgp` (and `.gpg`) files are binary - meaning they're faster and easier for computers to process, and they're more efficient in terms of storage. The problem with them, however, is that they aren't human readable, and occasionally this can mean you have fewer sharing options.<br /><br />`.asc`, on the other hand, can easily be opened in any text editor - and this also enables you to copy/paste the encrypted message directly into the body of an email for easy transmitting.

    When I receive the file, I will decrypt it using my <span style="color: var(--md-code-hl-special-color)">**private**</span> key, and save the resulting output as `secret-message.txt`. At this point, the file has been returned to a normal plain text file, unencrypted, and can easily be read by any text application on my machine.