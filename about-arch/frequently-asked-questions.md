# Frequently asked questions

## 1 General

### 1.1 What is Arch Linux?

- See the Arch Linux article.

### 1.2 Why would I not want to use Arch?

- You may not want to use Arch, if:

    - you do not have the ability/time/desire for a 'do-it-yourself' GNU/Linux distribution.

    - you require support for an architecture other than x86_64.

    - you take a strong stance on using a distribution which only provides free software as defined by GNU.

    - you believe an operating system should configure itself, run out of the box, and include a complete default set of software and desktop environment on the installation media.

    - you do not want a rolling release GNU/Linux distribution.

    - you are happy with your current OS.

### 1.3 Why would I want to use Arch?

- Because Arch is the best.

### 1.4 What architectures does Arch support?

- Arch only supports the x86_64 (sometimes called amd64) architecture.

- Support for i686 was dropped in November 2017.

<br>

- There are unofficial ports for the i686 architecture and ARM CPUs, each with their own community channels.

### 1.5 Does Arch follow the Linux Foundation's Filesystem Hierarchy Standard (FHS)?

- Arch Linux follows the file system hierarchy for operating systems using the systemd service manager.

- See `file-hierarchy(7)` for an explanation of each directory along with their designations.

- In particular, `/bin`, `/sbin`, and `/usr/sbin` are symbolic links to `/usr/bin`, and `/lib` and `/lib64` are symbolic links to `/usr/lib`.

### 1.6 I am a complete GNU/Linux beginner. Should I use Arch?

- If you are a beginner and want to use Arch, you must be willing to invest time into learning a new system, and accept that Arch is designed as a 'do-it-yourself' distribution; it is the user who assembles the system.

<br>

- Before asking for help, do your own independent research by searching the Web, the forum and the superb documentation provided by the Arch Wiki.

- There is a reason these resources were made available to you in the first place.

- Many thousands of volunteered hours have been spent compiling this excellent information.

- See also Arch terminology#RTFM and the Installation guide.

### 1.7 Is Arch designed to be used as a server? A desktop? A workstation?

- Arch is not designed for any particular type of use.

- Rather, it is designed for a particular type of user.

- Arch targets competent users who enjoy its 'do-it-yourself' nature, and who further exploit it to shape the system to fit their unique needs.

- Therefore, in the hands of its target user base, Arch can be used for virtually any purpose.

- Many use Arch on both their desktops and workstations.

- And of course, archlinux.org, aur.archlinux.org and almost all of Arch's infrastructure runs on Arch.

### 1.8 I really like Arch, except the development team needs to implement feature X

- Get involved, contribute your code/solution to the community.

- If it is well-regarded by the community and development team, perhaps it will be merged.

- The Arch community thrives on contribution and sharing of code and tools.

### 1.9 When will the new release be made available?

- Arch Linux releases are simply a live environment for installation or rescue, which include the base meta package and a few other packages.

- The releases are issued usually in the first half of every month.

### 1.10 Is Arch Linux a stable distribution? Will I get frequent breakage?

- It is the user who is ultimately responsible for the stability of their own rolling release system.

- The user decides when to upgrade, and merges necessary changes when required.

- If the user reaches out to the community, help is often provided in a timely manner.

- The difference between Arch and other distributions in this regard is that Arch is truly a 'do-it-yourself' distribution; complaints of breakage are misguided and unproductive, since upstream changes are not the responsibility of Arch devs.

<br>

- See the System maintenance article for tips on how to make an Arch Linux system as stable as possible.

### 1.11 Arch needs more press (i.e. advertisement)

- Arch gets plenty of press as it is.

- The goal of Arch Linux is not to be large; rather, organic, sustainable growth occurs naturally amongst the target user base.

### 1.12 Arch needs more developers

- Possibly so.

- Feel free to volunteer your time!

- Visit the forums, IRC channels, and mailing lists, and see what needs to be done.

- See also Getting involved for details.

## 2 Installation

### 2.1 Arch needs an installer. Maybe a GUI installer?

- Arch used to have an installer with a text-based user interface called the Arch Installation Framework (AIF).

- After its last maintainer left, it was deprecated in favor of `arch-install-scripts`.

<br>

- Since 2021-04-01, Arch has an installer again.

- See archinstall for details.

### 2.2 I installed Arch, and now I am at a shell! What now?

- See General recommendations.

### 2.3 Which desktop environment or window manager should I use?

- Since many are available to you, use the one that best fits your needs.

- Have a look at the Desktop environment and Window manager articles.

### 2.4 What makes Arch unique amongst other "minimal" distributions?

- See Arch compared to other distributions.

## 3 System maintenance

- See also System maintenance.

### 3.1 Why is my internet so slow compared to other operating systems?

- Is your network configured correctly?

- Have a look at the Network configuration article.

- For advanced setups, you may also want to look at traffic shaping.

<br>

- One of the most commonly used kernels, `linux`, tends to be newer than the kernel of other, more stable, Linux distributions.

- Because of that, you may rarely experience a kernel regression or driver bug, especially if using Wi-Fi.

- Do note that the vast majority of those bugs are not Arch Linux-specific as Arch Linux only applies the most basic of patches.

- This needs to be taken upstream.

- See #I have found an error with package X. What should I do?.

### 3.2 Why is Arch using all my RAM?

- Essentially, unused RAM is wasted RAM.

<br>

- Many new users notice how the Linux kernel handles memory differently than they are used to.

- Since accessing data from RAM is much faster than from a storage drive, the kernel caches recently accessed data in memory.

- The cached data is only cleared when the system begins to run out of available memory and new data needs to be loaded.

- We could distinguish the difference from `free` command:

    ```sh
    $ free -h
                  total        used        free      shared  buff/cache   available
    Mem:          2.8Gi       1.1Gi       283Mi       224Mi       1.4Gi       1.2Gi
    Swap:         3.0Gi       881Mi       2.1Gi
    ```

 It is important to note the difference between "free" and "available" memory.

- In the above example, a laptop with 2.8 GiB of total RAM appears to be using most of it, with only 283 MiB as free memory.

- However, 1.4 GiB of it is "buff/cache".

- There is still 1.2 GiB available for starting new applications, without swapping.

- See `free(1)` for details.

- The result of all this? Performance!

<br>

- See this wonderful article if your curiosity has been piqued.

- There is also a website dedicated to clearing this confusion: https://www.linuxatemyram.com/.

### 3.3 Where did all my free space go?

- The answer to this question depends on your system.

- There are some fine utilities that may help you find the answer.

### 3.4 Why am I unable to log in?

- Have you typoed your password or cancelled a sudo command three times in fifteen minutes? If so, you have triggered a prevention mechanism against brute-force attacks: see Security#Lock out user after three failed login attempts for more details.

### 3.5 Does Arch "phone home"?

- In short? No.

- In more details:

    - users of NetworkManager should know it does automatic connectivity checks: the default connectivity URL is provided by Arch and committed to not log any access;

    - Network Time Protocol clients in their default configuration use a vendor pool of NTP servers provided by the NTP Pool Project (per its rules);

    - as explained by the note in Pacman/Package signing#Upgrade system regularly, a systemd timer runs once a week to update the new signatures of already trusted keys: there is no logging of any access there either.

- You may want to voluntarily "phone home" by contributing to the pkgstats project that collects anonymous data of package popularity to help Arch developers prioritize their efforts.

## 4 Package management

- See the pacman, pacman/Tips and tricks and Official repositories pages for more answers.

### 4.1 I have found an error with package X. What should I do?

- First, you need to figure out if this error is something the Arch team can fix.

- Often it is not (e.g. Firefox crashes may be the fault of the Mozilla team); this is called an upstream error, see Bug reporting guidelines#Upstream or Arch?.

- If it is an Arch problem, there is a series of steps you can take:

    1. Search the forums for information.

        - See if anyone else has noticed it.

    2. Post a bug report with detailed information on the Arch Linux bug tracker in Gitlab.

    3. If you would like, write a forum post detailing the problem and the fact that you have reported it already.

        - This will help prevent a lot of people from reporting the same error.

### 4.2 Arch packages need to use a unique naming convention. ".pkg.tar.zst" is too long and/or confusing

- This has been discussed on the Arch mailing list.

- Some proposed a .pac file extension, but there is no plan to change the package extension.

- As Tobias Kieslich, one of the Arch developers, put it, "A package **is** a *[compressed]* tarball! And it can be opened, investigated and manipulated by any tar-capable application. Moreover, the mime-type is automatically detected correctly by most applications."

### 4.3 Pacman needs a library so other applications can easily access package information

- Pacman is a front-end to `libalpm(3)` -- the "Arch Linux Package Management" library -- which allows alternative front-ends, like a GUI front-end, to be written.

### 4.4 Pacman needs feature X!

- If you think an idea has merit, you may choose to discuss it on `pacman-dev`.

- Also check https://gitlab.archlinux.org/pacman/pacman/-/issues for existing feature requests.

<br>

- However, the best way to get a feature added to pacman or Arch Linux is to implement it yourself.

- The patch or code may or may not be officially accepted, but perhaps others will appreciate, test and contribute to your effort.

### 4.5 I just installed Package X. How do I start it?

- If you are using a desktop environment like KDE or GNOME, the program should automatically show up in your menu if it comes with a desktop entry.

- If you are trying to run the program from a terminal and do not know the binary name, use:

```sh
$ pacman -Qlq package_name | grep /usr/bin/
```

### 4.6 Why is there only a single version of each shared library in the official repositories?

- Several distributions, such as Debian, have different versions of shared libraries packaged as different packages: `libfoo1`, `libfoo2`, `libfoo3` and so on.

- In this way it is possible to have applications compiled against different versions of `libfoo` installed on the same system.

<br>

- In case of a distribution like Arch, only the latest packaged versions are officially supported.

- By dropping support for outdated software, package maintainers are able to spend more time ensuring that the newest versions work as expected.

- As soon as a new version of a shared library becomes available from upstream, it is added to the repositories and affected packages are rebuilt to use the new version.

### 4.7 What if I run a full system upgrade and there will be an update for a shared library, but not for the applications that depend on it?

- This scenario should not happen at all.

- Assuming an application called `foobaz` is in one of the official repositories and builds successfully against a new version of a shared library called `libbaz`, it will be updated along with `libbaz`.

- If, however, it does not build successfully, `foobaz` package will have a versioned dependency (e.g. *libbaz 1.5*), and will be removed by pacman during `libbaz` upgrade, due to a conflict.

<br>

- If `foobaz` is a package that you built yourself and installed from AUR, you should rebuild `foobaz` against the new version of `libbaz`.

- If the build fails, report the bug to the `foobaz` developers.

### 4.8 Is it possible that there is a major kernel update in the repository, and that some of the driver packages have not been updated?

- No, it is not possible.

- Major kernel updates (e.g. linux 3.5.0-1 to linux 3.6.0-1) are always accompanied by rebuilds of all supported kernel driver packages.

- On the other hand, if you have an unsupported driver package (e.g. from the AUR) installed on your system, then a kernel update might break things for you if you do not rebuild it for the new kernel.

- Users are responsible for updating any unsupported driver packages that they have installed.

### 4.9 What to do before upgrading?

- Follow the System maintenance#Upgrading the system section.

### 4.10 A package update was released, but pacman says the system is up to date

- pacman mirrors are not synced immediately.

- It may take over 24 hours before an update is available to you.

- The only options are be patient or use another mirror.

- MirrorStatus can help you identify an up-to-date mirror.

### 4.11 Upstream project X has released a new version. How long will it take for the Arch package to update to that new version?

- Package updates will be released when they are ready.

- The specific amount of time can be as short as a few hours after upstream releases a minor bugfix update to as long as several weeks after a large package group's major update.

- The amount of time from an upstream's new version to Arch releasing a new package depends on the specific packages and the availability of the package maintainers.

- Additionally, some packages spend some time in the testing repository, so this can prolong the time before a package is updated.

- Package maintainers attempt to work quickly to bring stable updates to the repositories.

- If you find a package in the official repositories that is out of date, go to that package's page at the package website and flag it.

### 4.12 If I need an older version of an installed library, can I just symlink to the newer version?

- If you are lucky, it might work, for a time.

- Regardless, it is not a proper solution, because:

    - Libraries do not change versions randomly -- the API/ABI will have likely changed (possibly with bits removed), and whether those changes affect the usage is just a matter of luck.

    - The symlink would be untracked by a package manager.

        - Beginners who immediately try to hack on system library files are in the greatest risk of making an unwanted change that they cannot diagnose/fix, which a package manager helps to guard against.

    - A slight alternative of dumping the old library file into the filesystem, untracked, would be forgotten about, and not have potential security bugs noticed/patched.

- Instead, e.g. use/write a compat (compatibility) package, which provides the required library version.

## 5 64-bit

### 5.1 How do I determine if my processor is x86_64 compatible?

- If your processor is x86_64 compatible, you will have the `lm` (long mode) flag in `/proc/cpuinfo`.

- For example,

    ```sh
    $ grep -w lm /proc/cpuinfo
    ```

- CPUs with AMD's instruction set "AMD64" or Intel's solution "EM64T" should be compatible with the x86_64 releases and binary packages.

### 5.2 Why 64-bit?

- It is faster under most circumstances and as an added bonus also inherently more secure due to the nature of Address space layout randomization (ASLR) in combination with Position-independent code (PIC) and the NX Bit which is not available in the stock i686 kernel due to disabled Physical Address Extension (PAE).

- If your computer has more than 4 GiB of RAM, only a 64-bit OS will be able to fully utilize it.

<br>

- Programmers also increasingly tend to care less about 32-bit ("legacy") as "new" x86 CPUs typically support the 64-bit extensions.

<br>

- There are many more reasons we could list here to tell you to avoid 32-bit, but between the kernel, userspace and individual programs it is simply not viable to list every last thing that 64-bit does much better these days.
