# install-docker-script
 
A one-command installer for **Docker CE** (official packages from Docker's own repository) on Kali Linux, Debian and Ubuntu.
 
Docker's official instructions and most install scripts fail on Kali because Kali reports its release codename as `kali-rolling`, which does not exist in Docker's repository. This script detects Kali automatically and uses the Debian `bookworm` repository instead, as Docker itself recommends.
 
## Supported platforms
 
| Architecture | Status |
|---|---|
| amd64 (x86_64) | ✅ |
| arm64 (aarch64), e.g. Kali on Apple Silicon (UTM/Parallels), Raspberry Pi 4/5 64-bit | ✅ |
| armhf (32-bit ARM) | ✅ |
 
| Distro | Docker repo used |
|---|---|
| Kali Linux | `debian/bookworm` |
| Debian / Raspberry Pi OS | system codename |
| Ubuntu / Linux Mint / Pop!_OS | Ubuntu codename |
 
## Usage
 
```bash
wget https://raw.githubusercontent.com/imbas007/install-docker-script/main/getdocker.sh -O getdocker.sh && bash getdocker.sh
```
 
> ⚠️ Do not run it with `sudo bash`. The script calls `sudo` itself. If you run it as root, your user will not be added to the `docker` group.
 
When it finishes, **log out and back in** (or run `newgrp docker`) so you can use `docker` without `sudo`:
 
```bash
docker ps
```
 
## What the script does
 
1. Detects the distro and CPU architecture, and exits early if either is unsupported
2. Removes old or conflicting Docker packages (`docker.io`, `docker-compose`, `podman-docker`, etc.)
3. Adds Docker's official GPG key and apt repository
4. Installs `docker-ce`, `docker-ce-cli`, `containerd.io`, `docker-buildx-plugin` and `docker-compose-plugin`
5. Enables and starts the Docker service
6. Adds the current user to the `docker` group
7. Runs `docker run hello-world` as a test
## Options
 
| Variable | Purpose | Example |
|---|---|---|
| `DOCKER_CODENAME` | Force a specific repo codename | `DOCKER_CODENAME=trixie bash getdocker.sh` |
| `SKIP_TEST` | Skip the `hello-world` test | `SKIP_TEST=1 bash getdocker.sh` |
 
## Troubleshooting
 
**`permission denied while trying to connect to the docker API`**
The new group membership is not active in your current session yet. Run `newgrp docker`, or log out and back in.
 
**apt errors about `kali-rolling` from a previous install attempt**
The script deletes the old `/etc/apt/sources.list.d/docker.list` automatically, so just run it again.
