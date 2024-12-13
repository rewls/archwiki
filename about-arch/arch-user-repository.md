# Arch User Repository

- The *Arch User Repository (AUR)* is a community-driven repository for Arch Linux users.

- It contains package descriptions (PKGBUILDs) that allow you to compile a package from source with makepkg and then install it via pacman.

- This document explains how users can access and utilize the AUR.

- A good number of new packages that enter the official repositories start in the AUR.

- If a package becomes popular enough it may be entered into the *extra* repository (directly accessible by *pacman* or from the Arch build system).

> ##### Warning
>
> - These `PKGBUILD`s are completely unofficial and have not been thoroughly vetted.

## 1 Getting started

- Users can search and download `PKGBUILD`s from the AUR Web Interface.

- These `PKGBUILD`s can be built into installable packages using *makepkg*, then installed using *pacman*.

- Ensure `base-devel` is installed.

- You may wish to adjust `/etc/makepkg.conf` to optimize the build process to your system prior to building packages from the AUR.

    - A significant improvement in package build times can be realized on systems with multi-core processors by adjusting the `MAKEFLAGS` variable, by using multiple cores for compression, or by using different compression algorithm.

    - See makepkg#Tips and tricks for more information.

## 2 Installing and upgrading packages

- Installing packages from the AUR is a relatively simple process.

- Essentially:

    1. Aquire the build files, including the PKGBUILD and possibly other required files, like systemd units and patches

    1. Verify that the `PKGBUILD` and accompanying files are not malicious or untrustworthy.

    1. Run `makepkg` in the directory where the files are saved.

        - This will download the code, compile it, and package it.

    1. Run `pacman -U package_file` to install the package onto your system.

### 2.2 Acquire build files

- There are several methods for acquiring the build files for a package:

    - Clone its git repository, labeled "Git Clone URL" in the "Package Details" on its AUR page.

        - This is the preferred method, an advantage of which is that you can easily get updates to the package via `git pull`.

### 2.3 Acquire a PGP public key if needed

- Check if a signature file in the form of *.sig* or *.asc* is part of the PKGBUILD source array.

- If that is the case, then acquire one of the public keys listed in the PKGBUILD validpgpkeys array.

- Refer to makepkg#Signature checking for more information. 

### 2.4 Build the package

- Change directories to the directory containing the package's PKGBUILD.

```shell
$ cd *package_name*
```

> ##### Warning
>
> - Carefully check the `PKGBUILD`, any *.install* files and any other files in the package's git repository for malicious or dangerous commands.
>
> - If in doubt, do not build the package, and seek advice on the forums or mailing list.

> ##### Tip
>
> - If you are updating a package, you may want to look at the changes since the last commit.
>
>   - To view changes since the last git commit, you can use `git show`.
>
>   - To view changes since the last commit using *vimdiff*, do `git diftool @~..@ --tool=vimdiff`.

- After manually confirming the contents of the files, run makepkg as a normal user

- Some helpful flags:

    - `-s`/`--syncdeps` automatically resolves and installs any dependencies with pacman becore building.

    - `-i`/`--install` installs the package if it is built successfully.

    - `-r`/`--rmdeps` removes built-time dependencies after the build, as they are no longer needed.

    - `-c`/`--clean` cleans up temporary build files after the build, so many are no longer needed.

> ##### Tip
>
> - Use `git clean -dfx` to delete all files that are not tracked by git, thus deleting all previously built package files.

### 2.5 Install the package

```shell
# pacman -U package_name-version-architecture.pkg.tar.zst
```

> ##### Note
>
> - If you have changed your `PKGEXT` in `makepkg.conf`, the name of the package file may be slightly different.
>
>  - It is highly recommended to read the makepkg and Arch build system articles for more details.

### 2.6 Upgrading packages

```shell
$ git pull
```

- Then follow the previous build and install instructions. 
