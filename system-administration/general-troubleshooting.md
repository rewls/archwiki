# General toubleshooting

## 2 Boot poblems

- When diagnosing boot problems, it is very important to know in which stage the boot fails.

	1. Firmware (UEFI or BIOS)

		1. USually only has very basic tools for debugging.

		2. Make sure Secure Boot is disabled.

	2. Boot loader

		1. One of the most common things done hee is changing of kernel parameters.

		2. Common boot issue during the boot loader stage could be caused by ACPI.

	3. initramfs

		1. Usually provides an emergency shell

		2. Depending on the hooks chosen, either the dmesg or the journal is available within it.

	4. The actual system

		1. Depending on how badly it is broken, a simple invocation of the debug shell may suffice here.

## 6 Package management

- See Pacman#Troubleshooting for general topics, and pacman/Package signing#Troubleshooting for issues with PGP keys.

### 6.1 Fixing a broken system

- If th system is broken enough that you are unable to run *pacman*, boot using a monthly Arch ISO from a USB flash drive, an optional disc or network with PXE.

	- Do not follow any of the rest of the installation guide.

- Mount your root file system:

	```shell
	[ISO] # mount /dev/*rootFileSystemDevice* /mnt
	```

- Mount any other partitions that you created separately, adding the prefix `/mnt` to all of them:

	```shell
	[ISO] # mount /dev/*bootDevice* /mnt/boot
	```

- Try using your system's *pacman*:

	```shell
	[ISO] # arch-chroot /mnt
	[chroot] # pacman -Syu
	```

- If that fails, exit the *chroot*, and try:

- If that fails, try:

	```shell
	[ISO] # pacman -Syu --root /mnt --cachedir /mnt/var/cache/pacman/pkg
	```
