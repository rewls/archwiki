# Kernel

- Arch Linux is based on the Linux kernel.

- There are various alternative Linux kernels available for Arch Linux in addition to the latest stable kernel.

- This article lists some of the options available in the repositories with a brief description of each.

- There is also a description of patches that can be applied to the system's kernel.

## 1 Officially supported kernels

- Stable — Vanilla Linux kernel and modules, with a few patches applied.

    - https://www.kernel.org/ || `linux`

## 2 Compilation

- Following methods can be used to compile your own kernel:

- /Arch Build System

    - Takes advantage of the high quality of existing linux PKGBUILD and the benefits of package management.

- /Traditional compilation

    - Involves manually downloading a source tarball, and compiling in your home directory as a normal user.

> ##### Warning
>
> - Using custom kernels may cause all kinds of stability and reliability issues, including data loss.
>
> - Having backups is strongly advised.

> ##### Tip
>
> - Best way to increase the speed of your system is to first tailor your kernel configuration to your architecture and processor type.
>
> - You can reduce the size of your kernel (and therefore build time) by not including support for things you do not have or use.
>
> - The `config` files for the Arch kernel packages are in the Arch package source files.
>
> - The `config` file of your currently running kernel may also be available in your file system at `/proc/config.gz` if the `CONFIG_IKCONFIG_PROC` kernel option is enabled.

- Some of the listed packages may also be available as binary packages via Unofficial user repositories.

### 2.1 kernel.org kernels

- Longterm 5.10 -- Long-term support (LTS) Linux 5.10 kernel an modules.

    - https://www.kernel.org/ || linux-lts510<sup>AUR</sup>
