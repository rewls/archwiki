# Installation guide

- This document is a guide for installing Arch Linux using the live system booted from an installation medium made from an official installation image.

- The installation medium provides accessibility features which are described on the page Install Arch Linux with accessibility options.

- Before installing, it would be advised to view the FAQ.

- For conventions used in this document, see Help:Reading.

- In particular, code examples may contain placeholders (formatted in `*italics*`) that must be replaced manually.

- Arch Linux should run on any x86_64-compatible machine with a minimum of 512 MiB RAM, though more memory is needed to boot the live system for installation.

- A basic installation should take less than 2 GiB of disk space.

- As the installation process needs to retrieve packages from a remote repository, this guide assumes a working internet connection is available.

## 1 Pre-installation

### 1.1 Acquire an installation image

- Visit the Download page and, depending on how you want to boot, acquire the ISO file or a netboot image, and the respective GnuPG signature.

### 1.2 Verify signature

- It is recommended to verify the image signature before use, especially when downloading from an *HTTP miror*, where downloads are generally prone to be intercepted to serve malicious.

- On a system with GnuPG installed, do this by downloading the *ISO PGP signature* to the ISO directory, and verifying it with:

	```shell
	$ gpg --keyserver-options auto-key-retrieve --verify archlinux-*version*-x86-64.iso.sig
	```

- Alternatively, from an existing Arch Linux installation run:

	```shell
	$ pacman-key -v archlinux-*version*-x86-64.iso.sig
	```

> ##### Note
>
> - The signature itself could be manipulated if it is downloaded from a mirror site, instead of from archlinux.org.
>
> - In this case, ensure that the public key, which is used to decode the signature, is signed by another, trustworthy key.
>
> - The `gpg` command will output the fingerprint of the public key.
>
> - Another method to verify the authenticity of the signature is to ensure that the public key's fingerprint is identical to the key fingerprint of the Arch Linux developer who signed the ISO-file.
>
> - See Wikipedia:Public-key cryptography for more information on the public-key process to authenticate keys.

### 1.3 Prepare an installation medium

- The ISO can be supplied to the target machine via a USB flash drive, an optical disc or a network with PXE: follow the appropriate article to prepare youself an installation medium from the ISO file.

- For the netboot image, follow Netboot#Boot fom a USB flash drive to prepare a USB flash drive for UEFI booting.

### 1.4 Boot the live environment

> ##### Note
>
> - Arch Linux installation images do not support Secure Boot.
>
> - You will need to disable Secure Boot to boot the installation medium.
>
> - If desired, Secure Boot can be set up after completing the installation.

1. Point the curret boot device to the one which has the Arch Linux installation medium.

2. When the installation medium's boot loade menu appears,

	- IF you used the ISO, select *Arch Linux install medium* and press `Enter` to enter the installation environment.

	> ##### Tip
	>
	> - The ISO uses systemd-boot for UEFI and syslinux for BIOS booting.
	>
	> - Use respectively `e` or `Tab` to enter the boot parameters.
	>
	> - A common example of manually defined boot parameter would be the font size.
	>
	> - For better readability on HiDPI screens -- when they are not already recognized as such -- using `fbcon=font:TER16x32` can help.
	>
	> - See HiDPI#Linux console (tty) for a detailed explanation.

- You will be logged in on the first virtual console as the root user, and presented with a Zsh shell prompt.

- To switch to a different console—for example, to view this guide with Lynx alongside the installation—use the `Alt+*arrow*` shortcut

- To edit configuration files, `mcedit(1)`, nano and vim are available.

- See pkglist.x86_64.txt for a list of the packages included in the installation medium.
