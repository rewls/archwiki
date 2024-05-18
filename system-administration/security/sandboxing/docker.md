# Docker

- Docker is a utility to pack, ship and run any application as a lightweight container.

## 1 Installation

- Topull Docker images and run Docker containers, you need the Docker Engine.

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

- If you want to be able to run the `docker` CLI command as a non-root user, add your user to the `docker user group, re-login, and restart `docker.service`.

> ##### Warning
>
> - Anyone added to the `docker` group is root equivalent because they can use the `docker run --privileged` command to start containers with root privileges.

### 1.1 Docker Compose

- Dlcoker Compose is an alternate CLI frontend for the Docker Engine, which specifies properties of containersusing a `docker-compose.yml` YAML file.

- To use it, install `docker-compose`.
