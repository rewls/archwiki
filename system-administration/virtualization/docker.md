# Docker

- Docker is a utility to pack, ship and run any application as a lightweight container.

## 1 Installation

- To pull Docker images and run Docker containers, you need the Docker Engine.

- The Docker Engine includes a daemon to manage the containers, as well as the `docker` CLI frontend.

- Install the `docker` package.

- Next enable/start `docker.service` or `docker.socket`.

- `docker.service` starts the service on boot, whereas `docker.socket` starts docker on first usage which can decrease boot times.

- Then verify docker's status:

    ```shell
    # docker info
    ```

- Next, verify that you can run containers.

- The following command downloads the latest Arch Linux image and uses it to run a Hello World program within a container:

    ```shell
    # docker run -it --rm archlinux bash -c "echo hello world"
    ```

- If you want to be able to run the `docker` CLI command as a non-root user, add your user to the `docker` user group, re-login, and restart `docker.service`.

> ##### Warning
>
> - Anyone added to the `docker` group is root equivalent because they can use the `docker run --privileged` command to start containers with root privileges.

### 1.1 Docker Compose

- Dlcoker Compose is an alternate CLI frontend for the Docker Engine, which specifies properties of containersusing a `docker-compose.yml` YAML file.

- To use it, install `docker-compose`.

### 1.2 Docker Desktop

- Docker Desktop is a proprietary desktop application that runs the Docker Engine inside a Linux virtual machine.

- Additional features such as a Kubernetes cluster and a vulnerability scanner are included.

- An experimental package for Arch is provided directly by Docker.

    - See https://docs.docker.com/desktop/install/archlinux/.

- Unfortunately, it contains files which conflict with the `docker-compose` and `docker-buildx` packages, so you will first need to remove them if installed.

- Alternatively, you can install docker-desktop<sup>AUR</sup> package that does not conflict with existing packages.

## 2 Usage

- It is possible to send requests to the Docker API and control the Docker daemon without the use of the docker CLI command.

    - See the Docker API developer documentation for more information, https://docs.docker.com/engine/api/.

- See the Docker Getting Started guide for more usage documentation, https://docs.docker.com/get-started/.

## 3 Configuration

- The Docker daemon can be configured either through a configuration file at `/etc/docker/daemon.json` or by adding command line flags to the `docker.service` systemd unit.

- According to the Docker official documentation, https://docs.docker.com/config/daemon/#configure-the-docker-daemon, the configuration file approach is preferred.

- If you wish to use the command line flags instead, use systemd drop-in files to override the `ExecStart` directive in `docker.service`.

- For more information about options in daemon.json see dockerd documentation, https://docs.docker.com/reference/cli/dockerd/#daemon-configuration-file.

### 3.4 Configuring DNS

- See Docker's DNS documentation, https://docs.docker.com/network/#dns-services for the documented behavior of DNS within Docker containers and information on customizing DNS configuration.

- In most cases, the resolvers configured on the host are also configured in the container.

- Most DNS resolvers hosted on `127.0.0.0/8` are not supported due to conflicts between the container and host network namespaces.

- Such resolvers are removed from the container's `/etc/resolv.conf`.

- If this would result in an empty /etc/resolv.conf, Google DNS is used instead.

- Additionally, a special case is handled if `127.0.0.53` is the only configured nameserver.

- In this case, Docker assumes the resolver is systemd-resolved and uses the upstream DNS resolvers from `/run/systemd/resolve/resolv.conf`.

- If you are using a service such as dnsmasq to provide a local resolver, consider adding a virtual interface with a link local IP address in the `169.254.0.0/16` block for dnsmasq to bind to instead of `127.0.0.1` to avoid the network namespace conflict.

## 4 Images

### 4.1 Arch Linux

- The following command pulls the archlinux x86_64 image.

```shell
# docker pull archlinux
```

- This is a stripped down version of Arch core without network, etc.

- See also https://gitlab.archlinux.org/archlinux/archlinux-docker/blob/master/README.md.

## 5 Tips and tricks

### 5.2 Run graphical programs inside a container

- This section describes the necessary steps to allow graphical programs to run on the host's X server.

- The container must be granted access to the host's X server.

- In a single-user environment, this can easily be done by running **Xhost** on the host system, which adds non-network local connections to the access control list:

    ```shell
    $ xhost + local:
    ```

- The following parameters need to be passed to `docker run`:

    - `-e "DISPLAY=$DISPLAY"` sets the environment variable `DISPLAY` within the container to the host's display;

    - `--mount type=bind, src=/tmp/.X11-unix,dst=/tmp/.X11-unix` mounts the host's X server sockets inside the container under the same path;

- To confirm that everything is set up correctly, run `glxgears` from the package `mesa-utils`, or `vkcube` from the package `vulkan-tools` in the container.

## 6 Remove Docker and images

- In case you want to remove Docker entirely you can do this by following the steps below:

- Check for running containers:

    ```shell
    # docker ps
    ```

- List all containers running on the host for deletion:

    ```shell
    # docker ps -a
    ```

- Stop a running container:

    ```shell
    # docker stop <CONTAINER ID>
    ```

- Killing still running containers:

    ```shell
    # docker kill <CONTAINER ID>
    ```

- Delete containers listed by ID:

    ```shell
    # docker rm <CONTAINER ID>
    ```

- List all Docker images:

    ```shell
    # docker images
    ```

- Delete images by ID:

    ```shell
    # docker rmi <IMAGE ID>
    ```

- Delete all images, containers, volumes, and networks that are not associated with a container (dangling):

    ```shell
    # docker system prune
    ```

- To additionally remove any stopped containers and all unused images (not just dangling ones), add the -a flag to the command:

    ```shell
    # docker system prune -a
    ```

- Delete all Docker data (purge directory):

    ```shell
    # rm -R /var/lib/docker
    ```

## 7 Troubleshooting

### 7.1 docker0 Bridge gets no IP / no internet access in containers when using systemd-networkd

- Docker attempts to enable IP forwarding globally, but by default systemd-networkd overrides the global sysctl setting for each defined network profile.

- Set `IPForward=yes` in the network profile.

- See Internet sharing#Enable packet forwarding for details.

> ##### Note
>
> - You may need to restart `docker.service` each time you restart `systemd-networkd.service` or `iptables.service`.
