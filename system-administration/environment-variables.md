# Environment variables

- An environment variable is a named object that contains data used by one or more applications.

- Environment variables provide a simple way to share configuration settings between multiple applications and processes in Linux.

## 1 Utilities

- The `coreutils` package contains the program *printenv* and *env*.

- To list the current environmental variables with values:

    ```shell
    $ printenv
    ```

> ##### Note
>
> - Some environment variables are user-specific.

- The *env* utility can be used to run a command under a modified environment.

- The shell builtin `set(1p)` allows you to change the values of shell options, set the positional parameters and to display the names and values of shell variables.

- Each process stores their environment in the `/proc/$PID/environ` file.

- This file contains each key value pair delimited by a nul character (`\x0`).

- A more human readable format can be obtained with sed, e.g. `sed 's:\x0:\n:g' /proc/$PID/environ`.

## 2 Defining variables

- To avoid needlessly polluting the environment, you should seek to restrict the scope of variables.

- The scopes of environment variables are broken down into the contexts they affect:

    - Global: All programs that any user runs, not including systemd services.

    - By user: All programs that a particular user runs, not including user systemd services (see Systemd/User#Environment variables) or graphical applications (see #Graphical envirionment).

### 2.1 Globally

#### 2.1.1 Using shell initialization files

- Most Linux distributions tell you to change or add environment variable definitions in `/etc/profile` or other locations.

- Keep in mind that there are also package-specific configuration files containing variable settings such as `/etc/locale.conf`.

- Be sure to maintain and manage the environment variables and pay attention to the numerous files that can contain environment variables.

- In principle, any shell script can be used for initializing environmental variables, but following traditional UNIX conventions, these statements should only be present in some particular files.

- The following files can be used for defining global environment variables on your system, each with different limitations:

    - `/etc/environment` is used by the pam_env module and is shell agnostic so scripting or glob expansion cannot be used.

        - The file only accepts `variable=value` pairs.

    - `/etc/profile` initializes variables for login shells *only*.

        - It does, however, run scripts (e.g. those in `/etc/profile.d/`) and can be used by all Bourne shell compatible shells.

    - Shell specific configuration files - Global configuration files of your shell,, initializes variables and runs scripts.

### 2.2 Per user

- Local environment variables can be defined in many different files:

    - User configuration files of your shell.

        - Unless you are restricting the scope of the variables to terminals you open, you are looking to modify the login shell.

    - systemd user environment variables are read from ``/.config/environment.d/*.conf`.

- To update the variable, re-login or source the file.

> ##### Tip
>
> - You can issue `export -p` to review the global and local environment variables declared for the user session.

### 2.3 Per session or shell

- One might want to temporarily run executables from a specific directory created without having to type the absolute path to each one, or use the path in a short temporary shell script.

- You can define the variable in your current shell, or use the *export* command to define it for all shells until you log out of the session.
