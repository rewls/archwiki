# GnuPG

- According to the official website:

    - GnuPG is a complete and free implementation of the OpenPGP standard as defined by RFC 4880 (also known as PGP).

    - GnuPG allows you to encrypt and sign your data and communications; it features a versatile key management system, along with access modules for all kinds of public key directories.

    - GnuPG, also known as GPG, is a command line took with features for easy integration with other applications.

    - A wealth of frontend applications and libraries are available.

    - GnuPG also provides support for S/MIME and Secure Shell (ssh).

## 1 Installation

- Install the `gnupg` package.

- This will also install `pinentry`, a collection of simple PIN or passphrase entry dialogs which GnuPG uses for passphrase entry.

- The shell script `/usr/bin/pinentry` determines which *pinentry* dialog is used, in the order described at pinentry.

## 3 Usage

> ##### Note
>
> - Whenever a `user-id` is required in a command, it can be specified with your key ID, fingerprint, a part of your name or email address, etc.
>
> - Whenever a `key-id` is needed, it can be found adding the `--keyid-format=long` flag to the command.
>
> - The *key-id* is the hexadecimal hash.

### 3.1 Create a key pair

- Generate a key pair by typing in a terminal:

    ```sh
    $ gpg --full-gen-key
    ```

- Also add the `--export` option to the command line to access more ciphers and in particular some newer elliptic curves like Curve448.

- The command will prompt for answers to several questions.

- For general use most people will want:

    - An expiration date: a period of one year is good enough for the average user.

        - This way even if access is lost to the keyring, it will allow others to know that it is no longer valid.

        - At a later stage, if necessary, the expiration date can be extended without having to re-issue a new key.

    - *no* optional comment.

        - Since the semantics of the comment field are not well-defined, it has limited value for identification.

    - A secure passphrase, find some guidelines in Security#Choosing secure passwords.

> ##### Note
>
> The name and email address you enter here will be seen by anybody who imports your key.

### 3.2 List keys

- To list keys in your public key ring:

    ```sh
    $ gpg --list-keys
    ```

- To list keys in your secret key ring:

    ```sh
    $ gpg --list-secret-keys
    ```

### 3.3 Export your public key

- GnuPG's main usage is to ensure confidentiality of exchanged messages via public-key cryptography.

- With it each user distributes the public key of their keyring, which can be used by others to encrypt messages to the user.

- The private key must *always* be kept private., otherwise confidentiality is broken.

- So, in order for others to send encrypted messages to you, they need your public key.

- To generate an ASCII version of user's public key to file `*public-key*.asc` (e.g. to distribute it by e-mail):

    ```sh
    $ gpg --export --armor --output *public-key*.asc *user-id*
    ```

> ##### Tip
>
> - Add `--no-emit-version` to avoid printing the version number, or add the corresponding setting to your `gpg.conf`.

### 3.4 Import a public key

- In order to encrypt messages to others, as well as verify their signatures, you need their public key.

- To import a public key with file name `*public-key*.asc` to your public key ring:

    ```sh
    $ gpg --import *public-key*.asc
    ```

- Alternatively, [**use a keyserver**](#35-use-a-keyserver) to find a public key.

<br>

- If you wish to import a key ID to install a specific Arch Linux package, see [**pacman/Package signing#Managing the keyring**]() and [**Makepkg#Signature checking**](https://github.com/rewls/archwiki/blob/main/about-arch/makepkg.md#13-signature-checking).

### 3.5 Use a keyserver

#### 3.5.2 Searching and receiving keys

- To find out details of a key on the keyserver, without importing it, do:

    ```sh
    $ gpg --search-keys user-id
    ```

- To import a key from a key server:

    ```sh
    $ gpg --receive-keys key-id
    ```

- To refresh/update the keychain with the latest version from a key server:

    ```sh
    $ gpg --refresh-keys
    ```

## 4 Key maintenance

### 4.1 Backup your private key

- To backup your private key do the following:

    ```sh
    $ gpg --export-secret-keys --armor --output *private-key*.asc *user-id*
    ```

- The above command will require that you enter the passphrase for the key.

> ##### Warning
>
> - The passphrase is usually the weakest link in protecting your secret key.
>
> - Place the private key in a safe place on a different system/device, such as a locked container or encrypted drive.
>
> - This method of backing up key has some security limitations.
>
> - See the [Moving GPG Keys Privately](https://vhs.codeberg.page/post/moving-gpg-keys-privately/) post on VHSblog for a potentially more secure way to back up and import keys using *gpg*.

- To import the backup of your private key:

    ```sh
    $ gpg --import *private-key*.asc
    ```

### 4.2 Backup your revocation certificate

- Revocation certificates are automatically generated for newly generated keys.

- These are by default located in `~/.gnupg/openpgp-revocs.d/`.

- The filename of the certificate is the fingerprint of the key it will revoke.

- The revocation certificates can also be generated manually by the user later using:

    ```sh
    $ gpg --gen-revoke --armor --output *revcert*.asc *user-id*
    ```

- This certificate can be used to #Revoke a key if it is ever lost or compromised.

- The backup will be useful if you have no longer access to the secret key and are therefore not able to generate a new revocation certificate with the above command.

> ##### Warning
>
> - Anyone with access to the revocation certificate can revoke the key publicly, this action cannot be undone.
>
> - Protect your revocation certificate like you protect your secret key.

### 4.3 Edit your key

- Running the `gpg --edit-key *user-id*` command will present a menu which enables you to do most of your key management related tasks.

- Type `help` in the edit key sub menu to show the complete list of commands.

- Some useful ones:

    ```sh
    > passwd    # change the passphrase
    > clean     # compact any user ID that is no longer usable (e.g revoked or expired)
    > revkey    # revoke a key
    > expire    # change the key expiration time
    ```

> ##### Tip
>
> - If you have multiple email accounts you can add each one of them as an identity, using `adduid` command.
>
> - You can then set your favourite one as `primary`.

### 4.5 Extending expiration date

> ##### Warning
>
> - **Never** delete your expired or revoked subkeys unless you have a good reason.
>
> - Doing so will cause you to lose the ability to decrypt files encrypted with t he old subkey.
>
> - Please **only** delete expired or revoked keys from other users to clean your keyring.

- When the key expires, it is relatively straight-forward to extend the expiration date:

    ```sh
    $ gpg --edit-key *user-id*
    > expire
    ```

- Finally, save the changes and quit:

    ```sh
    > save
    ```

- Alternatively, if you use this key on multiple computers, you can export the public key and import it on those machines.

- There is no need to re-export your secret key or update your backups: the master secret key itself never expires, and the signature of the expiration data left on the public key and subkeys is all that is needed.

### 4.7 Revoke a key

- Key revocation should be performed if the key is compromised, superseded, no longer used, or you forget your passphrase.

- This is done by merging the key with the revocation certificate of the key.

- To revoke the key, import the file saved in Backup your revocation certificate:

    ```sh
    $ gpg --import revcert.asc
    ```
