# Users and groups

- Users and groups are used on GNU/Linux for access control -- that is, to control access to the system's files, directories, and peripherals.

- Linux offers relatively simple/coarse access control mechanisms by default.

- For more advanced options, see ACL, Capabilities and PAM#Configuration How-Tos.

## 1 Overview

- A *user* is anyone who uses a computer.

- In this case, we are describing the names which represent those users.

- All that matters is that the computer has a name for each account it creates, and it is this name by which a person gains access to use the computer.

- Some system services also run using restricted or privileged user accounts.

<br>

- Managing users is done for the purpose of security by limiting access in certain specific ways.

- The superuser (root) has complete access to operating system and its configuration; it is intended for administrative use only.

- Unprivileged users can use several programs for controlled privilege elevation.

<br>

- Any individual may have more than one account as long as they use a different name for each account they create.

<br>

- Users may be grouped together into a "group", and users may be added to an existing group to utilize the privileged access it grants.

- Every file on a GNU/Linux system is owned by a user and a group.

- In addition, there are three types of access permissions: read, write, and execute.

- Different access permissions can be applied to a file's owning user, owning group, and others.

- One can be determine a file's owners and permissions by viewing the long listing format of the `ls` command:

	```sh
	$ ls -l /boot/
	total 13740
	drwxr-xr-x 2 root root    4096 Jan 12 00:33 grub
	-rw-r--r-- 1 root root 8570335 Jan 12 00:33 initramfs-linux-fallback.img
	-rw-r--r-- 1 root root 1821573 Jan 12 00:31 initramfs-linux.img
	-rw-r--r-- 1 root root 1457315 Jan  8 08:19 System.map26
	-rw-r--r-- 1 root root 2209920 Jan  8 08:19 vmlinuz-linux
	```

- The first column displays the file's permissions.

- The third and fourth columns display the file's owning user and group, respectively.

- It is also possible to determine a file's owners and permissions using the stat command:

- Owning user:

	```sh
	$ stat -c %U /media/sf_Shared/
	root
	```

- Owning group:

	```sh
	$ stat -c %G /media/sf_Shared/
	vboxsf
	```

- Access rights:

	```sh
	$ stat -c %A /media/sf_Shared/
	drwxrwx---
	```

- Access permissions are displayed in three groups of characters, representing the permissions of the owning user, owning group, and others, respectively.

- The first character represents the file's type.

<br>

- List files owned by a user or group with the find utility:

	```sh
	# find / -group *groupname*
	```

	```sh
	# find / -group *groupnumber*
	```

	```sh
	# find / -user *user*
	```

- A file's owning user and group can be changed with the `chown` (change owner) command.

- A file's access permissions can be changed with the `chmod` (change mode) command.

- See `chown(1)` and `chmod(1)` for additional detail.

## 4 File list

> ##### Warning
>
> - Do not edit these files by hand.
>
> - There are utilities that properly handle locking and avoid invalidating the format of the database.
>
> - See #User management and #Group management for an overview.

|File|Purpose|
|-|-|
|`/etc/passwd`|User account information|
|`/etc/group`|Defines the groups to which users belong|

## User management

- To list users currently logged on the system, the who command can be used.

- To list all existing user accounts including their properties stored in the user database, run `passwd -Sa` as root.

- See `passwd(1)` for the description of the output format.

<br>

- To add a new user, use the *useradd* command:

	```sh
	# useradd -m -G *additional_groups* -s *login_shell* *username*
	```

	- `-m`/`--create-home`

		- the user's home directory is created as `/home/*username*`.

		- The directory is populated by the files in the skeleton directory.

		- The created files are owned by the new user.

	- `-G`/`--groups`

		- a comma separated list of supplementary groups which the user is also a member of.

		- The default is for the user to belong only to the initial group.

	- `-s`/`--shell`

		- a path to the user's login shell.

		- Ensure the chosen shell is installed if choosing something other than Bash.

		- The default shell for newly created user can be set in `/etc/default/useradd`.

> ##### Warning
>
> - In order to be able to log in, the login shell must be one of those listed in `/etc/shells`, otherwise the PAM module `pam_shells(8)` will deny the login request.

> ##### Note
>
> - The password for the newly created user must then be defined, using `passwd` as shown in #Example adding a user.

- If an initial login group is specified by name or number, it must refer to an already existing group.

- If not specified, the behaviour of `useradd` will be depend on the `USERGROUPS_ENAB` variable contained in `/etc/login.defs`.

- The default behaviour (`USERGROUPS_ENAB yes`) is to create a group with the same name as the username.

<br>

- When the login shell is intended to be non-functional, for exmple when the user account is created for a specific service, `/usr/bin/nologin/` may be specified in place of a regular shell to politely refuse a login (see `nologin(8)).

- See `useradd(8)` for other supported options

## 8 Group management

- `/etc/group/` is the file that defines the groups on the system (see `group(5)` for details).

- Display group membership with the `groups` command:

	```sh
	$ groups *user*
	```

- If `user` is omitted, the current user's group names are displayed.

- The *id* command provides additional detail, such as the user's UID and associated GIDs:

	```sh
	$ id *user*
	```

- To list all groups on the system:

	```sh
	$ cat /etc/group
	```

- Create new groups with the `groupadd` command:

	```sh
	# groupadd *group*
	```

> ##### Note
>
> - If the user is currently logged in, they must log out and in again for changes to take effect.

- Add users to a group with the `gpasswd` command (see FS#58262 regarding errors):

	```sh
	# gpasswd -a *user* *group*
	```

- Alternatively, add a user to additional groups with `usermod` (replace `additional_groups` with a comma-separated list):

	```sh
	# usermod -aG *additional_groups* *username*
	```

> ##### Warning
>
> - If the `-a` option is omitted in the *usermod* command above, the user is removed from all groups not listed in `*additional_groups*`.

- Modify an existing group with the `groupmod` command, e.g. to rename the `*old_group*` group to `*new_group*`:

	```sh
	# groupmod -n *new_group* *old_group*
	```

> ##### Note
>
> - This will change a group name but not the numerical GID of the group.

- To delete existing groups:

	```sh
	# groupdel *group*
	```

- To remove users from a group:

	```sh
	# gpasswd -d *user* *group*
	```

- The `grpck` command can be used to verify the integrity of the system's group files.
