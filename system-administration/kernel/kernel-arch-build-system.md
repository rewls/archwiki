# Kernel/Arch build system

- The Arch build system can be used to build a custom kernel based on the official `linux` package.

- This compilation method can automate the entire process, and is based on a very well tested package.

- You can edit the PKGBUILD to use a custom kernel configuration or add additional patches

## 1 Getting the ingredients

```shell
$ mkdir ~/build/
$ cd ~/build/
```

- Install the `devtools` and `base-devel` package.

- Retrieve PKGBUILD source and few other files into your build directory by runnging:

	```shell
	$ pkgctl repo clone --protocol=https linux
	```

## 2 Modifying the PKGBUILD

- `PKGBUILD`

	```
	pkgbase=linux-custom
	```

### 2.1 Avoid creating the doc

- A large portion of the lengthy compiling effort is devoted to creating the documentation.

- As of 25 August 2022, the following patch to `PKGBUILD` avoids its creation:

	```
	63c63
	<   make htmldocs all
	---
	>   make all
	195c195
	< pkgname=("$pkgbase" "$pkgbase-headers" "$pkgbase-docs")
	---
	> pkgname=("$pkgbase" "$pkgbase-headers")
	```

- This patch changes lines #63 and #195.

- You might have to edit the `PKGBUILD` file manually if it does not apply cleanly.

### Changing prepare()

- In `prepare()` function, you can apply needed kernel patches or change kernel build configuration.

- Since 2018-08-01, the PKGBUILD automatically applies all `*.patch` files in source.

- If you need to change a few configuration options you can edit `config` in the source.

> ##### Warning
>
> - systemd has a number of kernel configuration requirements for general use, for specific usecases (e.g., UEFI) and specific systemd functionality (e.g., bootchar).
>
> - The list of requied and recommended kernel CONFIGs can be found in `/usr/share/doc/systemd/README`.
>
> - Check them before you compile.
>
> - These requirements also change over time.
>
> - Because Arch assumes you are using the official kernel, there will be no announcement of these changes.
>
> - Before you install a new version of systemd, check the version release notes to make sure your current custom kernel meets any new systemd requirements.

### 2.3 Generate new checksums

- #Changing prepare() suggests a possible modification to `$_srcname/.config`.

- Since this path is not where downloading the package files ended, its checksum was not checked by makepkg (which actually checked `$_srcname/../../config`).

- If you replaced the downloaded `config` with another one before running makepkg, install the `pacman-contrib` package and generate new checksums by running:

	```shell
	$ updpkgsums
	```

## 3 Compiling

```shell
$ makepkg -s
```

- The `-s` parameter will download any additional dependencies used by recent kernels such as xml and odcs.

> ##### Note
>
> - Kernel sources are PGP signed, and makepkg will attempt to verify them.
>
> - See makepkg#Signatue checking for details.
>
> - The compilation can take up to several hours to complete depending on the hardware performance.
>
> - Running compilation jobs simultaneously can reduce compilation time significantly on multi-core systems.
>
> - It can be informative to run the above `makepkg` using the `time` command to know how long your system took to perform the compilation.

## 4 Installing

- The compile step will leave two packages in the `~/build/linux` folder, one for the kernel and one for the kernel heades.

- Best practice is to install both packages together as they might be both needed:

	```shell
	# pacman -U linux-custom-headers-5.8.12-x86_64.pkg.tar.zst linux-custom-5.8.12-x86_64.pkg.tar.zst
	```

## 5 Boot loader

- If you have modified `pkgbase` in order to have your new kernel installed alongside the default kernel you will need to update your boot loader configuration file and add new entries for you custom kernel and the associated initramfs images.
