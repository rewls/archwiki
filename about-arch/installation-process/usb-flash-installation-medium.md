# USB flash installation medium

- This page discusses various multi-platform methods on how to create an Arch Linux Installer USB drive (also referred to as "flash drive", "USB stick", "USB key", etc) for booting in BIOS and UEFI systems.

- The result will be a live USB system that can be used for installing Arch Linux, system maintenance or for recovery purposes, and that, because of using Overlayfs for `/`, will discard all changes once the computer shuts down.

<br>

- If you would like to run a full install of Arch Linux from a USB drive (i.e. with persistent settings), see [**Install Arch Linux on a removable medium**]().

- If you would like to use your bootable Arch Linux USB stick as a rescue USB, see [**chroot**]().

- Before following any of these steps, download the ISO from [***https://archlinux.org/download/***](https://archlinux.org/download/) and verify its integrity.

## 1 Using the ISO as is (BIOS and UEFI)

> ##### Warning
>
> - This will irrevocably delete all data on your USB flash drive, so make sure you do not have any important files on the flash driver before doing this.

> ##### Note
>
> - If, instead of a USB flash drive or an SD card, you want to write the ISO to a hard disk drive or a solid state drive, make sure the drive's logical sector size is not larger than 2048 bytes (the ISO 9660 sector size) and aligns to it.
>
> - This means the ISO cannot be written to a 4Kn Advanced Format drive using this method.

### 1.2 In Windows

#### 1.2.5 Using Rufus

- Rufus is a multi-purpose USB ISO writer.

- It provides a graphical user interface and does not care if the drive is properly formatted or not.

<br>

- Simply select the Arch Linux ISO, the USB drive you want to create the bootable Arch Linux onto and click START.

> ##### Note
>
> - If the USB drive does not boot properly using the default ISO Image mode, **DD Image mode** should be used instead.
>
> - To switch this mode on, select *GPT* from the *Partition scheme* drop-down menu.
>
> - After clicking *START* you will get the mode selection dialog, select *DD Image mode*.

> ##### Tip
>
> - To add an additional partition for persistent storage use the slider to choose the persistent partition's size.
>
> - When using the persistent partition feature, make sure to select *MBR* in the *Partition scheme* drop-down menu and *BIOS* or *UEFI* in *Target System*, otherwise the drive will not be usable for both BIOS and UEFI booting.
