# makepkg

- [***makepkg***]() is a script to automate the building of packages.

- The requirements for using the script are a build-capable [***Unix***]() platform and a [PKGBUILD]().

<br>

- *makepkg* is provided by the [**`pacman`**]() package.

## 1 Configuration

### 1.3 Signature checking

> ##### Note
>
> - The signature checking implemented in *makepkg* does not use pacman's keyring, instead relying on the user's keyring.

- If a signature file in the form of *.sig* or *.asc* is part of the [**PKGBUILD**]() source array, *makepkg* automatically attempts to verify it.

- In case the user's keyring does not contain the needed public key for signature verification, *makepkg* will abort the installation with a message that the PGP key could not be verified.

<br>

- If a needed public key for a package is missing, the [**PKGBUILD**]() will most likely contain a [**validpgpkeys**]() entry with the required key IDs.

- [**Import**](https://github.com/rewls/archwiki/blob/main/system-administration/security/cryptography/encryption/gnupg.md#34-import-a-public-key) it manually, or find it [**on a keyserver**](https://github.com/rewls/archwiki/blob/main/system-administration/security/cryptography/encryption/gnupg.md#35-use-a-keyserver) and import it from there.

- To temporarily disable signature checking, run makepkg with the `--skippgpcheck` option.
