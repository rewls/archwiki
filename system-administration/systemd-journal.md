# systemd/Journal

- *systemd* has its own loggin system called the journal; running a separate logging daemon is not required.

- To read the log, use `journalctl(1)`.

- In Arch Linux, the directory `/var/log/journal/` is a part of the `systemd` package, and the journal (when `Storage=` is set to `auto` in `/etc/systemd/journald.conf`) will write to `/var/log/journal/`.

- If that directory is deleted, *systemd* will **not** recreate it automatically and instead will write its logs to `/run/systemd/journal` in a nonpersistent way.

- However, the directory will be recreated if `Storage=persistent` is added to `journald.conf` and `systemd-journald.service` is restarted.

- systemd journal classifies messages by Priority level and Facility.

- Loggin classification corresponds to classic Syslog protocol (RFC 5424).
