# fdisk

- util-linux fdisk is a dialogue-driven command-line utility that creates and manipulates partition tables and partitions on a hard disk.

- Hard disks are divided into partitions and this division is described in the partition table.

<br>

- This article covers [**`fdisk(8)`**]() and its related [**`sfdisk(8)`**]() utility.

> ##### Note
>
> - *fdisk* supports GPT since [**`util-linux`**]() 2.23.
>
> - Alternatively, [**`gptfdisk`**]() may be used; see [**gdisk**]() for more information.

> ##### Tip
>
> - For basic partitioning functionality with a curses-based user interface, [**`cfdisk(8)`**]() can be used.

## 1 Installation

- *fdisk* and its associated utilities are provided by the [**`util-linux`**]() package, which is a dependency of the [**`base`**]() meta package.

## 2 List partitions

- To list partition tables and partitions on a block device, you can run the following, where device is a name like `/dev/sda`, `/dev/nvme0n1`, `/dev/mmcblk0`, etc.:

    ```sh
    # fdisk -l /dev/sda
    ```

> ##### Note
>
> - If the device is not specified, *fdisk* will list all partitions in `/proc/partitions`.

## 3 Backup and restore partition table

- Before making changes to a hard disk, you may want to backup the partition table and partition scheme of the drive.

- You can also use a backup to copy the same partition layout to numerous drives.

<br>

- For both GPT and MBR you can use *sfdisk* to save the partition layout of your device to a file with the `-d`/`--dump` option.

- Run the following command for device `/dev/sda`:

```sh
# sfdisk -d /dev/sda > sda.dump
```

- The file should look something like this for a single ext4 partition that is 1 GiB in size:

    - `sda.dump`:

        ```
        label: gpt
        label-id: AAAAAAAA-BBBB-CCCC-DDDD-EEEEEEEEEEEE
        device: /dev/sda
        unit: sectors
        first-lba: 34
        last-lba: 1048576
        sector-size: 512

        /dev/sda1 : start=2048, size=1048576, type=0FC63DAF-8483-4772-8E79-3D69D8477DE4, uuid=BBF1CD36-9262-463E-A4FB-81E32C12BDE7
        ```

- To later restore this layout you can run:

    ```sh
    # sfdisk /dev/sda < sda.dump
    ```

## 4 Create a partition table and partitions

- The first step to partitioning a disk is making a partition table.

- After that, the actual partitions are created according to the desired partition scheme.

- See the [**partition table**]() article to help decide whether to use MBR or GPT.

<br>

- Before beginning, you may wish to backup your current partition table and scheme.

<br>

- *fdisk* performs partition alignment automatically on a 2048 512-byte sector (1 MiB) block size base which should be compatible with all Advanced Format HDDs and the vast majority of SSDs if not all.

- This means that the default settings will give you proper alignment.

<br>

- To use *fdisk*, run the program with the name of the block device you want to change/edit.

- This example uses `/dev/sda`:

    ```sh
    # fdisk /dev/sda
    ```

- This opens the *fdisk* dialogue where you can type in commands.

### 4.1 Create new table

> ##### Warning
>
> - If you create a new partition table on a disk with data on it, it will erase all the data on the disk.
>
> - Make sure this is what you want to do.

> ##### Tip
>
> - Check that your NVMe drives and Advanced Format hard disk drives are using the optimal logical sector size before partitioning.
>
> - Consider performing SSD memory cell clearing before partitioning an SSD.

- To create a new partition table and clear all current partition data type `g` at the prompt for a GUID Partition Table (GPT) or `o` for an MBR partition table.

- Skip this step if the table you require has already been created.

### 4.2 Create partitions

- Create a new partition with the `n` command.

- You must enter a partition number, starting sector, and an ending sector.

- If formatting with MBR, it may also require a partition type.

> ##### Note
>
> - See [**Partitioning#Partition scheme**]() for considerations concerning the size and location of partitions.

#### 4.2.1 Partition type

- When using MBR, *fdisk* will ask for the MBR partition type.

- Specify it, type `p` to create a primary partition or `e` to create an extended one.

- There may be up to four primary partitions.

<br>

- *fdisk* does not ask for the partition type ID and uses 'Linux filesystem' by default; you can change it later.

### 4.2.2 Partition number

- A partition number is the number assigned to a partition, e.g. a partition with number 1 on a disk `/dev/sda` would be `/dev/sda1`, `/dev/nvme0n1p1` on `/dev/nvme0n1` and `/dev/mmcblk0p1` on `/dev/mmcblk0`.

- See [**Device file#Partition**]() for details on the naming scheme.

- Partition numbers may not always match the order of partitions on disk, in which case they can be sorted.

<br>

- It is advised to choose the default number suggested by *fdisk*.

### 4.2.3 First and last sector

- The first sector must be specified in absolute terms using sector numbers.

- The last sector can be specified using the absolute position in sector numbers or as positions measured in kibibytes (`K`), mebibytes (`M`), gibibytes (`G`), tebibytes (`T`), or pebibytes (`P`);

<br>

- The position of the last sector can be specified in:

    - absolute terms from the start of the disk. E.g. `40M` as a first sector specifies a position 40 MiB from the start of the disk.

    - relative terms by preceding the size with `+size` or `-size`.

        - E.g. `+2G` to specify a point 2 GiB after the start sector, or `-200M` to specify a point 200 MiB before the last available sector.

- Pressing the Enter key with no input specifies the default value, which is the start of the largest available block for the first sector and the end of the same block for the last sector.

> ##### Note
>
> - When partitioning it is always a good idea to follow the default value for a partition's first sector.
>
>   - Additionally, make sure to specify partition sizes with the `+size{M,G,T,P}` notation and do not use sizes smaller than 1 MiB.
>
>   - Such partitions will always be aligned according to the device properties.
>
> - EFI system partition requires the partition type EFI System.
>
> - GRUB requires a BIOS boot partition with partition type BIOS boot when installing GRUB to a GPT partitioned disk on a BIOS-based system.

> ##### Tip
>
> - On a disk with an MBR partition table leave at least 33 512-byte sectors (16.5 KiB) of free unpartitioned space at the end of the disk to allow converting between MBR and GPT.

- Repeat this procedure until you have the partitions you desire.

### 4.3 Change partition type

- Each partition is associated with a type.

- MBR uses partition IDs; GPT uses Partition type GUIDs.

<br>

- Press `t` to change the type of a partition.

- You can use an alias for common partition types:

    - `uefi` for the ESP

    - `xbootldr` for an XBOOTLDR partition

    - `home` for the home partition

    - `swap` for the swap partition

    - `linux` for other partitions

> ##### Tip
>
> - Press `L` to show fdisk's internal code list.
>
> - When using GPT, it is advised to follow the [***Discoverable Partitions Specification***].
>
> - This allows using GPT automounting.

### 4.4 Make an MBR partition bootable

- You can make an MBR partition bootable (aka active) by typing `a`.

### 4.5 Review and write changes to the partition table

- Print changes via the `p` command.

- Abort changes via the `q` command.

- Write changes to disk and exit via the `w` command.

### 4.6  Create a file system

- To create a file system on the new partition, see [**File systems#Create a file system**]().

## 5 Moving partitions

> ##### Warning
>
> - Partitions can only be moved while offline.
>
> - Because moving a partition requires the whole partition to be rewritten on disk, it is a slow and potentially hazardous operation.
>
> - Backups are strongly recommended!
>
> - According [**`sfdisk(8) § OPTIONS`**](), "this operation is risky and not atomic."

- In order to move a partition, you need to have free space available where the partition will be moved.

- If necessary, you can make room by shrinking your partitions and the filesystems on them.

- See [**Parted#Shrinking partitions**]().

- To relocate a partition:

    ```sh
    # echo '+*sectors*,' | sfdisk --move-data *device* -N *number*
    ```

- Where *`sectors`* is the number of sectors to move the partition (the `+` indicates moving it forward), *`device`* is the device that holds the partition, and *`number`* is the partition number.

- Note that if you add a new partition in the middle or at the beginning of your disk, you will likely want to renumber the partitions.

- See [**#Sort partitions**]() or the "extra functionality" mode of *fdisk*.

## 6 Tips and tricks

### 6.1 Sort partitions

- This applies for when a new partition is created in the space between two partitions or a partition is deleted.

- `/dev/sda` is used in this example.

    ```sh
    # sfdisk -r /dev/sda
    ```

- After sorting the partitions if you are not using Persistent block device naming, it might be required to adjust the `/etc/fstab` and/or the `/etc/crypttab` configuration files.

> ##### Note
>
> - The kernel must read the new partition table for the partitions (e.g. `/dev/sda1`) to be usable.
>
> - Reboot the system or tell the kernel to reread the partition table (i.e. `partprobe /dev/sda`).

## 7 See also

- [***Managing partitions in Linux with fdisk***]()
