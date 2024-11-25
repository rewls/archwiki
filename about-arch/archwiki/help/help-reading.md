# Help:Reading

- Because the vast majority of the ArchWiki contains indications that may need clarification for users new to Arch Linux (or GNU/Linux in general), this rundown of basic procedures was written both to avoid confusion in the assimilation of the articles and to deter repetition in the content itself.

## 1 Organization

- Most articles on the ArchWiki do not attempt to provide a holistic introduction to a single topic; they are instead written in adherence to the "Don't Repeat Yourself" principle, under the assumption that the user will seek out and read any supporting material that they do not yet understand.

- Where possible, such supporting material is indicated in the article via special formatting, see [**#Formatting**](#2-formatting).

<br>

- Because of this organization, it may be necessary to examine several related sources in order to fully understand an ArchWiki article.

- In particular, users who are new to Arch (or GNU/Linux in general) should expect to end up reading a great number of articles even when solving simple problems.

- It is especially important to study the supporting material before seeking additional help from other users.

## 2 Formatting

- link to a section in the current article: [**#Organization**](#1-organization)

- link to [**another ArchWiki article**]()

- link to an [***external web page***]()

- link to a man page: [**`intro(1)`**]()

- a man page that's only available offline: `foo(1)`

- link to a package in the official repositories: [**`foobar`**]()

- link to a package in the AUR: [**`foobar`**]()<sup>AUR</sup>

## 3 Root, regular user or another user

- Some lines are written like so:

    ```sh
    # mkinitcpio -p linux
    ```

- Others have a different prefix:

    ```sh
    $ makepkg -s
    ```

- The numeral or hash sign (`#`) indicates that the command needs to be run as root, whereas the dollar sign (`$`) shows that the command should be run as a regular user.

> ##### Note
>
> - The commands prefixed with `#` are intended to be executed from a root shell, which can for example be easily accessed with `sudo -i`.
>
> - Running <code>sudo *command*</code> from an unprivileged shell instead of *`command`* from a root shell will also work in most cases, with some notable exceptions such as redirection and command substitution, which strictly require a root shell.
>
> - See [**sudo#Login shell**]().

- When the commands need to run as a specific user, they will be prefixed by the username in square brackets, for example:

    ```sh
    [postgres]$ initdb -D /var/lib/postgres/data
    ```

- This means you should use a privilege elevation tool, e.g. with sudo:

    ```sh
    $ sudo -u postgres initdb -D /var/lib/postgres/data
    ```

- A notable exception to watch out for:

    ```sh
    # This alias makes ls colorize the listing
    alias ls='ls --color=auto'
    ```

- In this example, the context surrounding the numeral sign communicates that this is not to be run as a command; it should be edited into a file instead.

- So in this case, the numeral sign denotes a comment.

- A comment can be explanatory text that will not be interpreted by the associated program.

- Bash scripts denotation for comments happens to coincide with the root *PS1*.

<br>

- After further examination, "give away" signs include the uppercase character following the `#` sign.

- Usually, Unix commands are not written this way and most of the time they are short abbreviations instead of full-blown English words (e.g., Copy becomes cp).

<br>

- Regardless, most articles make this easy to discern by notifying the reader:

- *Append* to `~/path/to/file`:

    ```sh
    # This alias makes ls colorize the listing
    alias ls='ls --color=auto'
    ```

## 4 Append, add, create, edit

- When prompted to append to, add to, create, or edit one or more files, it is implied that you should use one of the following methods.

<br>

- To create or modify multiline files, it is suggested to use a text editor.

> ##### Note
>
> - Text files must end with a newline because a line is terminated with a newline.
>
> - Most text editors insert an ending newline by default.

- To create or overwrite a file from a string, it may be simpler to use output redirection.

- The following example creates or overwrites the contents of the file `/etc/hostname` with the text `myhostname`.

    ```sh
    # echo myhostname > /etc/hostname
    ```

- Output redirection can also be used to append a string to a file.

- The following example appends the text `[custom-repo]` to the file `/etc/pacman.conf`.

    ```sh
    # echo "[custom-repo]" >> /etc/pacman.conf
    ```

- When prompted to create directories, use the mkdir command:

    ```sh
    # mkdir /mnt/boot
    ```

### 4.1 Make executable

- After creating a file, if it is meant to be run as a script (whether manually or called by another program), it needs to be set as executable, for example with:

    ```sh
    $ chmod +x *script*
    ```

- See [**chmod**]().

- Some applications such as file managers may provide graphical interfaces to do the same.

## 5 Source

- Some applications, notably command-line shells, use scripts for their configuration: after modifying them, they must be *sourced* in order for the changes to be applied.

- In the case of bash, for example, this is done by running (you can also replace `source` with `.`):

    ```sh
    $ source ~/.bashrc
    ```

- When the wiki suggests modifying such a configuration script, it will not explicitly remind you to source the file, and only in some cases will it point to this section with a reminder link.

## 6 Installation of packages

- When an article invites you to install some packages in the conventional way, it will not indicate the detailed instructions to do so; instead, it will simply mention the names of the packages to be installed.

> ##### Note
>
> - Frequently, the install or installed links are used to point to this article section.
>
> - However, JavaScript has to be enabled for these links to work.

- The subsections below give an overview of the generic installation procedures depending on the package type.

### 6.1 Official packages

- For packages from the official repositories, you will read something like:

    - Install the [**`foobar`**]() package.

- This means that you have to run:

    ```sh
    # pacman -S foobar
    ```

- The **pacman** article contains detailed explanations to deal with package management in Arch Linux proficiently.

### 6.2 Arch User Repository

- For packages from the Arch User Repository (AUR), you will read something like:

    - Install the [**`foobar`**]()<sup>AUR</sup> package.

- This means that in general you have to follow the [**`foobar`**]()<sup>AUR</sup> link, download the PKGBUILD archive, extract it, **verify the content** and finally run, in the same folder:

    ```sh
    $ makepkg -si
    ```

> ##### Note
>
> - The [**base-devel**]() meta package is required to build packages from the AUR or with the Arch build system.

- [**The Arch User Repository**] article contains all the detailed explanations and best practices to deal with AUR packages.

## 7 Control of systemd units

- When an article invites to *start*, *enable*, etc., some systemd unit (e.g. a service), it will not indicate the detailed instructions to do so, but instead you will read something like:

    - [**Start**]() `example.service`.

- This means that you have to run:

    ```sh
    # systemctl start example.service
    ```

- A notable command that does not follow this exact pattern is *daemon-reload* which will be called without arguments.

<br>

- The [**systemd#Using units**]() section contains structured list of available actions (like start, enable, enable and start, etc.) with their corresponding *systemctl* commands.

## 8 System-wide versus user-specific configuration

- It is important to remember that there are two different kinds of configurations on a GNU/Linux system.

- **System-wide** configuration affects all users.

- Since system-wide settings are generally located in the `/etc` directory, root privileges are required in order to alter them.

- For example, to apply a Bash setting that affects all users, `/etc/bash.bashrc` should be modified.

<br>

- **User-specific** configuration affects only a single user.

- *Dotfiles* are used for user-specific configuration.

- For example, the file `~/.bashrc` is the user-specific configuration file.

- The idea is that each user can define their own settings, such as aliases, functions and other interactive features like the prompt, without affecting other users' preferences.

> ##### Note
>
> - `~/` and `$HOME` are shortcuts for the user's home directory, usually `/home/username/`.

### 8.1 Common shell files

- Bash and other Bourne-compatible shells, such as Zsh, also source files depending on whether the shell is a *login shell* or an *interactive shell*.

- See [**Bash#Configuration files**]() and [**Zsh#Startup/Shutdown files**]() for details.

## 9 Pseudo-variables in code examples

- Some code blocks may contain so-called pseudo-variables, which, as the name says, are not actual variables used in the code.

- Instead they are generic placeholders and have to be manually replaced with system-specific configuration items **before** the code may be run or parsed.

- Common shells such as bash and zsh provide tab-completion to auto-complete parameters for common commands such as *systemctl*.

<br>

- In the articles that comply with [**Help:Style/Formatting and punctuation**](), *pseudo-variables* are formatted in italics.

- For example:

    - Enable the <code>dhcpcd@*interface_name*.service</code> for the network interface identified from the output of the `ip link` command.

- In this case *`interface_name`* is used as a pseudo-variable placeholder in a systemd template unit.

- All systemd template units, identifiable by the `@` sign, require a system-specific configuration item as argument.

- See [**systemd#Using units**]().

    - The command <code>dd if=*data_source* of=/dev/sd*X* bs=*sector_size* count=*sector_number* seek=*partitions_start_sector*</code> can be run as root to wipe a partition with the specific parameters.

- In this case the *pseudo-variables* are used to describe the parameters that must be substituted for them.

- Details on how to gather them are elaborated on in the section [**Securely wipe disk#Calculate blocks to wipe manually**](), which features the command.

<br>

- In case of file examples, pasting pseudo-variables in real configuration files might break the programs that use them.

### 9.1 Ellipses

- In most cases, ellipses (`...`) are not part of the actual file content or code output, and instead represent omitted or optional text that is not relevant for the discussed subject.

- For example `HOOKS="... encrypt ... filesystems ..."` or:

    - `/etc/X11/xorg.conf.d/50-synaptics.conf`:

        ```
        Section "InputClass"
            ...
            Option      "CircularScrolling"          "on"
            Option      "CircScrollTrigger"          "0"
            ...
        EndSection
        ```

- Be aware though that, in a few instances, ellipses may be a meaningful part of the code syntax: attentive users will be able to easily recognize these cases by the context.
