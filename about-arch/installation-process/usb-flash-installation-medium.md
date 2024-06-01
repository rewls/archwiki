# USB flash installation medium

- This page discusses various multi-platform methods on how to create an Arch Linux Installer USB drive for booting in BIOS and UEFI systems.

- Before following any of these steps, download the ISO from https://archlinux.org/download/ and verify its integrity.

## 1 Using the ISO as is (BIOS and UEFI)

> ##### Warning
>
> - THis will irrevocably delete all data on your USB flash drive, so make sure you do not have any important files on the flash driver before doing this.

### 1.2 In Windows

#### 1.2.1 Using KDE ISO Image Writer

- KDE ISO Image Writer can be downloaded as *.exe* file at isoimagewriter.

- It can auto-detect the USB-drive and you need to manually select a ISO file.

- It is recommended to use *.sig* file to signature but it can be skipped by clicking "create".
