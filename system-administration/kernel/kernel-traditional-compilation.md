# Kernel/Traditional compilation

- This article is an introduction to building custom kernels from kernel.org sources.

- This method of compiling kernels is the traditional method common to all distributions.

## 1 Preparation

- It is not necessary (or recommended) to use the root account or root privileges for kernel preparation.

### 1.1 Install the core packages

- Install the `base-devel` meta package, which pulls in necessary packages such as `make` and `gcc`.

- It is also recommended to install the following packages, as listed in the default Arch kernel PKGBUILD: `xmlto`, `kmod`, `inetutils`, `bc`, `libelf`, `git`, `cpio`, `perl`, `tar`, `xz`.

### 1.2 Create a kernel compilation directory

- It is recommended to create a separate build directory for your kernel(s).

```shell
$ mkdir ~/kernelbuild
```

### 1.3 Download the kernel source

> ##### Warning
>
> - systemd requires kernel version 3.12 at least (4.2 or greater for unified cgroups hierarchy support).
>
> - See /usr/share/doc/systemd/README for more information.

- Download the kernel source from https://www.kernel.org.

- This should be the tarball (.tar.xz) file for your chosen kernel.

> ##### Note
>
> - It is a good idea to verify the PGP signature of any downloaded kernel tarball.
>
> - See kernel.org/signature.

```shell
$ cd ~/kernelbuild
$ wget https://cdn.kernel.org/pub/linux/kernel/v*A*.x/linux-*A*.*B*.*C*.tar.xz
```

- First grab the signature, then use that to grab the fingerprint of the signing key, then use the fingerprint to obtain the actual signing key:

```
$ wget https://cdn.kernel.org/pub/linux/kernel/v*A*.x/linux-*A*.*B*.*C*.tar.sign
$ gpg --list-packets linux-*A*.*B*.*C*.tar.sign | grep -i keyid | awk '{print $NF}' | xargs gpg --recv-keys
```

- The signature was generated for the tar archive.

```shell
$ unxz linux-*A*.*B*.*C*.tar.xz
$ gpg --verify linux-*A*.*B*.*C*.tar.sign linux-*A*.*B*.*C*.tar
```

- Do not proceed if this does not result in output that includes the string "Good signature".

## 1.4 Unpack the kernel source

```shell
$ tar -xvf linux-*A*.*B*.*C*.tar
```

- To be absolutely sure that no permission errors occur, chown needs to be run to transfer ownership of the folder to the current user.

```shell
$ chown -R $USER:$USER linux-*A*.*B*.*C* 
```

- To finalise the preparation, ensure that the kernel tree is absolutely clean; do not rely on the source tree being clean after unpacking.

- To do so, first change into the new kernel source directory created, and then run the `make mrproper` command:

```shell
$ cd linux-*A*.*B*.*C*
$ make mrproper
```

> ##### Note
>
> - The `mrproper` Make target depends on the clean target, and thus, it is not necessary to execute both.

## 2 Kernel configuration

- Kernel configuration is set in its `.config` file, which includes the use of Kernel modules.

> ##### Note
>
> - It is not necessary to use the root account or root privileges at this stage.

- You can do a mixture of two things:

    - Use the default Arch settings from an official kernel (recommended)

    - Manually configure the kernel options (optional, advanced and not recommended)

### 2.1 Default Arch configuration

- If a stock Arch kernel is running, you can use the following command inside the custom kernel source directory:

    ```shell
    $ zcat /proc/config.gz > .config
    ```

- Otherwise, the default configuration can be found online in the official Arch Linux kernel package, https://gitlab.archlinux.org/archlinux/packaging/packages/linux/-/blob/main/config.

> ##### Tip
>
> - If you are upgrading kernels, some options may have changed or been removed.
>
> - In this case, when running `make` under #Compilation, you will be asked to provide answers to every configuration option that has changed between versions.
>
> - To accept the defaults without being prompted, run `make olddefconfig`.

- If you are compiling a kernel using your current `.config` file, do not forget to rename your kernel version "CONFIG_LOCALVERSION" in the new `.config`.
>
> - If you skip this, there is the risk of overwriting one of your existing kernels by mistake.

## 3 Compilation

- Compilation time will vary from as little as fifteen minutes to over an hour, depending on your kernel configuration and processor capability.

```shell
$ make
```

> ##### Tip
>
> - To compile faster, make can be run with the `-j*X*` argument, where `*X*` is an integer number of parallel jobs.
>
> - The best results are often achieved using the number of CPU cores in the machine; for example, with a 2-core processor run `make -j2`.
>
> - See Makepkg#Improving build times for more information.

## 4 Installation

### 4.1 Install the modules

- Once the kernel has been compiled, the modules for it must follow. First build the modules:

    ```shell
    $ make modules
    ```

- Then install the modules.

- As root or with root privileges, run the following command to do so:

    ```shell
    # make modules_install
    ```

- This will copy the compiled modules into `/lib/modules/A.B.C/`.

- This keeps the modules for individual kernels used separated.

### 4.2 Copy the kernel to /boot directory

> ##### Note
>
> - Ensure that the `bzImage` kernel file has been copied from the appropriate directory for your system architecture. See below.

- The kernel compilation process will generate a compressed `bzImage` (big zImage) of that kernel, if it does not, you may have to run

    ```shell
    make bzImgage
    ```

- This file must be copied to the `/boot` directory and renamed in the process.

- Provided the name is prefixed with `vmlinuz-`, you may name the kernel as you wish.

```shell
# cp -v arch/x86/boot/bzImage /boot/vmlinuz-linux*A**B*
```

### 4.3 Make initial RAM disk

> ##### Note
>
> - You are free to name the initramfs image file whatever you wish when generating it.
>
> - However, it is recommended to use the `linux*major_revision**minor_revision*` convention.

- If you do not know what making an initial RAM disk is, see Initramfs on Wikipedia and mkinitcpio.

#### 4.3.1 Automated preset method

- An existing mkinitcpio preset can be copied and modified so that the custom kernel initramfs images can be generated in the same way as for an official kernel.

- This is useful where intending to recompile the kernel (e.g. where updated).

- In the example below, the preset file for the stock Arch kernel will be copied and modified for kernel *A*.*B*.*C*, installed above.

- First, copy the existing preset file, renaming it to match the name of the custom kernel specified as a suffix to `/boot/vmlinuz-` when copying the `bzImage`:

    ```shell
    # cp /etc/mkinitcpio.d/linux.preset /etc/mkinitcpio.d/linux*A**B*.preset
    ```

- Second, edit the file and amend for the custom kernel.

- Note (again) that the `ALL_kver=` parameter also matches the name of the custom kernel specified when copying the bzImage:

    - `/etc/mkinitcpio.d/linux*A**B*.preset`

        ```
        ...
        ALL_kver="/boot/vmlinuz-linux*A**B*"
        ...
        default_image="/boot/initramfs-linux*A**B*.img"
        ...
        fallback_image="/boot/initramfs-linux*A**B*-fallback.img"
        ```

- Finally, generate the initramfs images for the custom kernel in the same way as for an official kernel:

    ```shell
    # mkinitcpio -p linux*A**B*
    ```

#### 4.3.2 Manual method

- Rather than use a preset file, mkinitcpio can also be used to generate an initramfs file manually. The syntax of the command is:

    ```shell
    # mkinitcpio -k *kernel_version* -g /boot/initramfs-*file_name*.img
    ```

    - `-k` (`--kernel *kernel_version*`)

        - Specifies the modules to use when generating the initramfs image.

        - The `kernel_version` name will be the same as the name the modules directory for it, located in `/usr/lib/modules/` (alternatively, a path to the kernel image can be used).

    - `-g` (`--generate *file_name*`)

        - Specifies the name of the initramfs file to generate in the `/boot` directory.

        - Again, using the naming convention mentioned above is recommended.

### 4.4 Copy System.map

- The `System.map` file is not required for booting Linux.

- It is a type of "phone directory" list of functions in a particular build of a kernel.

- The `System.map` contains a list of kernel symbols and their corresponding addresses.

- This "symbol-name to address mapping" is used by:

    - Some processes like klogd, ksymoops, etc.

    - By OOPS handler when information has to be dumped to the screen during a kernel crash (i.e info like in which function it has crashed).

> ##### Tip
>
> - EFI system partitions are formatted using FAT32, which does not support symlinks.

- If your `/boot` is on a filesystem which supports symlinks, copy `System.map` to `/boot`, appending your kernel's name to the destination file.

- Then create a symlink from `/boot/System.map` to point to `/boot/System.map-linuxAB`:

```shell
# cp System.map /boot/System.map-linux*A**B*
# ln -sf /boot/System.map-linux*A**B* /boot/System.map
```

- After completing all steps above, you should have the following 3 files and 1 soft symlink in your `/boot` directory along with any other previously existing files:

- Kernel: `vmlinuz-linux*A**B*`

- Initramfs: `initramfs-linux*A**B*.img`

- System Map: `System.map-linux*A**B*`

- System Map kernel symlink: `System.map` (which symlinks to `System.map-linux*A**B*`)

## 5 Bootloader configuration

- Add an entry for your new kernel in your bootloader's configuration file.

- See Arch boot process#Feature comparison for possible boot loaders, their wiki articles and other information.

> ##### Tip
>
> - Kernel sources include a script to automate the process for LILO: it can be safely ignored if you are using an other bootloader.
