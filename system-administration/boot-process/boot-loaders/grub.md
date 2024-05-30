# GRUB

- GRUB (GRand Unified Bootloader) is a boot loader.

- The current GRUP is also referred to as GRUB 2.mo

- This page exclusively describes GRUB 2.

## 4 Configuration

- On an installed system, GRUB loads the `/boot/grub/grub.cfg` configuration file each boot.

- You can follow #Generated grub.cfg for using a tool, or #Custom grub.cfg for a manual creation.

### 4.1 Generated grub.cfg

- This section only covers editing the /etc/default/grub configuration file. See /Tips and tricks for more information.

> ##### Note
>
> - Remember to always re-generate the main configuration file after making changes to /etc/default/grub and/or files in /etc/grub.d/.

> ##### Warning
>
> - Update/reinstall the boot loader (see #UEFI systems or #BIOS systems) if a new GRUB version changes the syntax of the configuration file: mismatching configuration can result in an unbootable system.

#### 4.1.1 Generate the main configuration file

- After the installation, the main configuration file `/boot/grub/grub.cfg` needs to be generated.

- The generation process can be influenced by a variety of options in `/etc/default/grub` and scripts in `/etc/grub.d/`.

- For the list of options in `/etc/default/grub` and a concise description of each refer to GNU's documentation.

- If you have not done additional configuration, the automatic generation will determine the root filesystem of the system to boot for the configuration file.

- For that to succeed it is important that the system is either booted or chrooted into.

- Use the *grub-mkconfig* tool to generate `/boot/grub/grub.cfg`:

    ```shell
    # grub-mkconfig -o /boot/grub/grub.cfg
    ```

- By default the generation scripts automatically add menu entries for all installed Arch Linux kernels to the generated configuration.

> ##### Tip:
>
> - After installing or removing a kernel, you just need to re-run the above *grub-mkconfig* command.
>
> - For tips on managing multiple GRUB entries, for example when using both `linux` and `linux-lts` kernels, see /Tips and tricks#Multiple entries.
