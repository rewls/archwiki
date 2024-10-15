# systemd/User

- systemd offers the ability to manage services under the user's control with a per-user systemd instance, enabling them to start, stop, enable, and disable their own **user units**.

- This is convenient for daemons and other services that are commonly run for a single user, such as mpd, or to perform automated tasks like fetching mail.

## 1 How it works

- As per default configuration in `/etc/pam.d/system-login`, the `pam_systemd` module automatically launches a `systemd --user` instance when the user logs in for the first time.

- This process will survive as long as there is some session for that user, and will be killed as soon as the last session for the user is closed.

- The systemd user instance is responsible for managing user services, which can be used to run daemons or automated tasks with all the benefits of systemd, such as socket activation, timers, dependency system, and strict process control via cgroups.

<br>

- Similar to system units, user units are located in the following directories (ordered by ascending precedence):

    - `/usr/lib/systemd/user/` where units provided by installed packages belong.

    - `~/.local/share/systemd/user/` where units of packages that have bee installed in the home directory belong.

    - `/etc/systemd/user/` where system-wide user units are placed by the system administrator.

    - `~/.config/systemd/user/` where the user puts their own units.

- When a systemd user instance starts, it brings up the per user target `default.target`.

- Other units can be controlled manually with `systemctl --user`.

- See `systemd.special(7) § UNITS MANAGED BY THE USER SERVICE MANAGER`.

> ##### Note
>
> - Be aware that the `systemd --user` instance is a per-user process, and not per-session.
>
>   - The rationale is that most resources handled by user services, like sockets or state files will be per-user and not per session.
>
>   - As a consequence, programs that need to be run inside a session will probably break in user services.
>
>   - The way systemd handles user sessions is pretty much in flux.
>
> - `systemd --user` runs as a separate process from the `systemd --system` process.

- User units can not reference or depend on system units or units of other users.

## 2 Basic setup

- All the user units will be placed in `~/.config/systemd/user/`.

- If you want to start units on first login, execute `systemctl --user enable *unit*` for any unit you want to be autostarted.

> ##### Tip
>
> - If you want to enable a unit for all users rather than the user executing the *systemctl* command, run `systemctl --global enable *unit*` as root.
>
> - Similarly for `disable`.

## 2.1 Environment variables

- Units started by user instance of systemd do not inherit any of the environment variables set in places like `.bashrc` etc.

### 2.1.2 Systemd user instance

- The above only addresses default environment variables for userunits.

- However, the systemd user instance itself is also affected by some environment variables.

- In particular, certain specifiers (see `systemd.unit(5) § SPECIFIERS`) are affected by XDG variables.

<br>

- however, the systemd user instance will *only* use environment variables that are set when it is started.

- Therefore, if such environment variables are needed ,they should be set in a drop in configuration file, see #Service example.

<br>

- Systemd does not provide introspection tools to check these values, however, something like the following service can be used to help checking that the specifiers expand as expected:

- `$XDG_CONFIG_HOME/systemd/user/test-specifiers.service`

    ```
    [Service]
    Type=onechot
    ExecStart=printf '(systemd)=(envvar)\n'
    ExecStart=printf '%%s=%%s\n' %C "${XDG_CACHE_HOME}"
    ExecStart=printf '%%s=%%s\n' %E "${XDG_CONFIG_HOME}"
    ExecStart=printf '%%s=%%s\n' %L "${XDG_STATE_HOME}"/log
    ExecStart=printf '%%s=%%s\n' %S "${XDG_STATE_HOME}"
    ExecStart=printf '%%s=%%s\n' %t "${XDG_RUNTIME_DIR}"
    ```

### 2.1.2 Service example

- Create the drop-in directory `/etc/systemd/system/user@.service.d/` and inside create a file that has the extension *.conf*:

- `/etc/systemd/system/user@.service.d/local.conf`

    ```
    [Service]
    Environment="PATH=/usr/lib/ccache/bin:/usr/local/sbin:/usr/local/bin:/usr/bin"
    Environment="EDITOR=nano -c"
    Environment="BROWSER=firefox"
    Environment="NO_AT_BRIDGE=1"
    Environment="XDG_STATE_HOME=%h/.local/var/state"
    ```

## 3 Writing user units

- See systemd#Writing unit files for general information about writing systemd unit files.

### 3.1 Example

- The following is an example of a user version of the mpd service:

- `~/.config/systemd/user/mpd.service`

    ```
    [Unit]
    Description=Music Player Daemon

    [Service]
    ExecStart=/usr/bin/mpd --no-daemon

    [Install]
    WantedBy=default.target
    ```

## 3.3 Reading the journal

- The journal for the user can be read using the analogous command:

    ```sh
    $ journalctl  --user
    ```

- To specify a unit, one can use

    ```sh
    $ journalctl --user-unit myunit.service
    ```

- Or equivalently:

    ```sh
    $ journalctl --user -u myunit.service
    ```

> ##### Note
>
> - journald will not write user journals for users with UIDs below 1000, instead directing everything to the system journal.
