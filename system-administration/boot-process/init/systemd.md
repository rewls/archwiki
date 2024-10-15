# systemd

- From the project web page:

> - *systemd* is a suite of basic building blocks for a Linux system.
>
> - It provides a system and service manager that runs as PID 1 and starts the rest of the system.
>
> - *systemd* provides aggressive parallelization capabilities, uses socket and D-Bus activation for starting services, offers on-demand starting of daemons, keeps track of processes using Linux control groups, maintains mount and automount points, and implements an elaborate transactional dependency-based service control logic.
>
> - *systemd* supports SysV and LSB init scripts and works as a replacement for sysvinit.
>
> - Other parts include a logging daemon, utilities to control basic system configuration like the hostname, date, locale, maintain a list of logged-in users and running containers and virtual machines, system accounts, runtime directories and settings, and daemons to manage simple network configuration, network time synchronization, log forwarding, and name resolution.

- Historically, what systemd calls "service" was named daemon: any program that runs as a "background" process, commonly waiting for events to occur and offering services.

- For more information see `daemon(7)`.

## 1 Basic systemctl usage

- The main command used to introspect and control *systemd* is *systemctl*.

- Some of its uses are examining the system state and managing the system and services.

- See `systemctl(1)` for more details.

> ##### Tip
>
> - You can use all of the following *systemctl* commands with the `-H user@host` switch to control a *systemd* instance on a remote machine.
>
>   - This will use SSH to connect to the remote *systemd* instance.

### 1.1 Using units

- Units commonly include, but are not limited to, services (*.service*), mount points (*.mount*), devices (*.device*) and sockets (*.socket*).

<br>

- When using *systemctl*, you generally have to specify the complete name of the unit file, including its suffix.

- There are however a few short forms when specifying the unit in the following *systemctl* commands:

    - If you do not specify the suffix, systemctl will assume *.service*.

    - Mount points will automatically be translated into the appropriate *.mount* unit.

        - For example, `netctl` and `netctl.serviced are equivalent.

    - Similar to mount points, devices are automatically translated into the appropriate *.devcie* unit, therefore specifying `/dev/sda2` is equivalent to `dev-sda2.device`.

- See `systemd.unit(5)` for details.

> ##### Note
>
> - Some unit names contain an `@` sign (e.g. `name@string.service`): this means that they are instances of a *template* unit, whose actual file name does not contain `string` part.
>
> - `string` is called the *instance identifier*, and is similar to an argument that is passed to the template unit when called with the *systemctl* command: in the untit file it will substitute the `%i`specifier.
>
> - To be more accurate, *before* trying to instantiate the `name@.suffix` template unit, *systemd* will actually look for a unit with the exact `name@string.suffix` file name, although by convention such a "clash" happens rarely, i.e. most unit files containing an `@` sign are meant to be templates.
>
> - Also, if a template units is called without an instance identifier, it will generally fail (except with certain *systemctl* commands, like `cat`).

- The commands in the below table operate on **system units** since `--system` is the implied default for *systemctl*.

- To instead operate on **user units**, use `systemctl --user` without root privileges.

- See also systemd/User#Basic setup to enable/disable user units for *all users* to enable/disable user units for *all users*..

> ##### Tip
>
> - Most commands also work if multiple units are specified, see `systemctl(1)` for more information.
>
> - The `--now` switch can be used in conjunction with `enable`, `disable`, and `mask` to respectively start, stop, or mask the unit *immediately* rather than after rebooting.
>
> - A package may offer units for different purposes.
>
>   - If you just installed a package, `pacman -Qql package | grep -Fe .service -e .socket` can be used to check and find them.

- Analyzing the system state:

    |Action|Command|Note|
    |-|-|-|
    |Show system status|`systemctl status`|
    |List running units|`systemctl` or `systemctl list-units`|
    |List failed units|`systemctl --failed`|
    |List installed unit files|`systemctl list-unit-files`|
    |Show process status for a PID|`systemctl status *pid*`|cgroup slice, memory and parent|

- Checking the unit status:

    |Action|Command|Note|
    |-|-|-|
    |Show a manual page associated with a unit|`systemctl help *unit*`|as supported by the unit|
    |Status of a unit|`systemctl status *unit*`|including whether it is running or not|
    |Check whether a unit is enabled|`systemctl is-enabled *unit*`|

- Starting, restarting, reloading a unit

    |Action|Command|Note|
    |-|-|-|
    |Start a unit immediately|`systemctl start unit` as root|
    |Stop a unit immediately|`systemctl stop *unit*` as root|
    |Restart a unit|`systemctl restart *unit*` as root|
    |Reload a unit and its configuration|`systemctl reload *unit*` as root|
    |Reload systemd manager configuration|`systemctl daemon-reload` as root|scan for new or changed units|

- Enabling a unit

    |Action|Command|Note|
    |-|-|-|
    |Enable a unit to start automatically at boot|`systemctl enable *unit*` as root|
    |Enable a unit to start automatically at boot and start it immediately|`systemctl enable --now *unit*` as root|
    |Disable a unit to no longer start at boot|`systemctl disable *unit*` as root|
    |Reenable a unit|`systemctl reenable *unit*`as root|i.e. disable and enable anew|

- Masking a unit

    |Action|Command|Note|
    |-|-|-|
    |Mask a unit to make it impossible to start|`systemctl mask *unit*` as root|
    |Unmask a unit|`systemctl unmask *unit*` as root|

1. See `system.unit(5) § UNIT FILE LOAD PATH` for the directories where available unit files can be found.

2. This does not ask the changed units to reload their own configurations (see the Reload action).

3. For example, in case its `[Install]` sectio nhas changed since last enabling it.

4. Both manually and as a dependency, which makes masking dangerous.

    - Check for existing masked units with:

    ```sh
    $ systemctl list-unit-files --state=masked
    ```

## 1.2 Power management

- polkit is necessary for power management as an unprivileged user.

- If you are in a local *systemd-logined* user session and no other session is active, the following commands will work without root privileges.

- If not, *systemd* will automatically ask you for the root password.

|Action|Command|
|-|-|
|Shut down and reboot the system|`system reboot`|
|Shut down and power-off the system|`systemctl poweroff`|
|Suspend the system|`systemctl suspend`|
|Put the system into hibernation (write RAM to disk)|`systemctl hibernate`|
|Put the system into hybrid-sleep state (also called suspend-to-both, it saves RAM to disk and then suspends)|`systemctl hybrid-sleep`|
|First suspend the system, then wake up after a configured time in order to just hibernate the system|`systemctl suspend-then-hibernate`|
|Perform a reboot of the userspace-only with a #Soft reboot.|`systemcrl soft-reboot`|

### 1.2.1 Soft reboot

- Soft reboot is a special kind of a userspace-only reboot operation that does not involve the kernel.

- It is implemented by `systemd-soft-reboot.service(8)` and can be invoked through `systemctl soft-reboot`.

- As with kexec, it skips firmware re-initialization, but additionally the systme does not go through kernel initialization and initramfs, and unlocked dm-crypt devices remain attached.

## 2 Writing unit files

- The syntax of *systemd*'s unit files (`systemd.unit(5)`) is inspired by XDG Desktop Entry Specification .desktop files, which are in turn inspired by Microsoft Windows .ini files.

- Unit files are loaded from multiple locations (to see the full list, run `systemctl show --property=UnitPath`), but the main ones are (listed fro mlowest to highest precedence):

    - `/usr/lib/systemd/system/`: units provided by installed packages

    - `/etc/systemd/system/`: units installed by the system administrator

> ##### Note:
>
> - The load paths are completely different when running *systemd* in usermode.
>
> - *systemd* unit names may only contain ASCII alphanumeric characters, underscores and periods.
>
>   - All other characters must be replaced by C-style "\x2d" escapes, or employ their predefined semantics ('@', '-').
>
>   - See `systemd.unit(5)` and `systemd-escape(1)` for more information.

- Look at the units installed by your packages of examples, as well as `systemd.service(5) § EXAMPLES`.

> ##### Tip
>
> - Comments prepended with `#` may be used in unit-files as well, but only in new lines.
>
> - Do not use end-line comments after *systemd* parameters or the unit will fail to activate.

- `systemd-analyze(1)` can help verifying the work.

- See the `systemd-analyze verify FILE...` section of that page.

### 2.1 Handling dependencies

- With *systemd*, dependencies can be resolved by designing the unit fiels correctly.

- The most typical case is when unit *A* requires unit *B* to be running before $A$ is started.

- In that case add `Requires=*B*` and `After=*B*` to the `[Unit]` section of *A*.

- If the dependency is optional, add `Wants=*B*` and `After=*B*` instead.

- Note that `Wants=` and `Requires=` do not imply `After=`, meaning that if `After=` is not specified, the two units will be started in parallel.

<br>

- Dependencies are typically placed on services and not on #Targets.

## 2.2 Service types

- There are several different start-up types to consider when writing a custom service file.

- This is set with the `Type=` parameter in the `[Service]` section:

    - `Type=simple` (default): *systemd* considers the service to be started up immediately.

        - The process must not fork.

        - Do not use this type if other services need to be ordered on this service, unless it is socket activated.

    - `Type=forking`: *systemd* considers the service started up once the process forks and the parent has exited.

        - For classic daemons, use this type unless you know that it is not necessary.

        - You should specify `PIDFile=` as well so *systemd* can keep track of the main process.

    - `Type=oneshot`: this is useful for scripts that do a single job and then exit.

        - You may want to set `RemainAfterExit=yes` as well so that *systemd* still considers the service as active after the process has exited.

        - Setting `RemainAfterExit=yes` is appropriate for the units which change the system state (e.g., mount some partition).

- See the `systemd.service(5) § OPTIONS` man page for a more detailed explanation of the `Type` values.

## 3 Targets

- *systemd* uses *targets* to group units together via dependencies and as standardized synchronization points.

- Each *target* is named and is intended to serve a specific purpose with the possibility of having multiple ones active at the same time.

- Some *targets* are implemented by inheriting all of the services of another *target* and adding additional services to it.

### 3.1 Get current targets

```sh
$ systemctl list-units --type=target
```
