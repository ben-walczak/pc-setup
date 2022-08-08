# Docker

What?

Why?

## Docker Engine
"Docker Engine is an open source containerization technology for building and containerizing your applications. Docker Engine acts as a client-server application with:

- A server with a long-running daemon process `dockerd`.
- APIs which specify interfaces that programs can use to talk to and instruct the Docker daemon.
- A command line interface (CLI) client `docker`.

The CLI uses [Docker APIs](https://docs.docker.com/engine/api/) to control or interact with the Docker daemon through scripting or direct CLI commands. Many other Docker applications use the underlying API and CLI. The daemon creates and manage Docker objects, such as images, containers, networks, and volumes.

For more details, see [Docker Architecture](https://docs.docker.com/get-started/overview/#docker-architecture)."
- [docker docs](https://docs.docker.com/engine/)

### Installation
Following [this](https://docs.docker.com/engine/install/ubuntu/) guide from docker docs.

### Install repository

1. Update `apt` and install packages to allow `apt` to use a repo over HTTPS
```bash
sudo apt-get update
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```

2. Add Docker's official GPG key
```bash
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

3. Use the following command to set up the repository
```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

### Post Install

**Manage Docker as a non-root user**

"The Docker daemon binds to a Unix socket instead of a TCP port. By default that Unix socket is owned by the user `root` and other users can only access it using `sudo`. The Docker daemon always runs as the `root` user.

If you don’t want to preface the `docker` command with `sudo`, create a Unix group called `docker` and add users to it. When the Docker daemon starts, it creates a Unix socket accessible by members of the `docker` group." - [docker docs
](https://docs.docker.com/engine/install/linux-postinstall/)

1. Create the `docker` group
```bash
sudo groupadd docker
```

2. Add your user to the docker group
```bash
sudo usermod -aG docker $USER
```

3. Log out and log back in so that your group membership is re-evaluated.
```bash
newgrp docker
```

4. Verify that you can run `docker` commands without `sudo`.
```bash
docker run hello-world
```

If any errors occured please visit the [docs](https://docs.docker.com/engine/install/linux-postinstall/)

**Configure Docker to start on boot**
Most current Linux distributions (RHEL, CentOS, Fedora, Debian, Ubuntu 16.04 and higher) use `systemd` to manage which services start when the system boots. On Debian and Ubuntu, the Docker service is configured to start on boot by default. To automatically start Docker and Containerd on boot for other distros, use the commands below:
```bash
sudo systemctl enable docker.service
sudo systemctl enable containerd.service
```
To disable this behavior, use `disable` instead
```bash
sudo systemctl disable docker.service
sudo systemctl disable containerd.service
```