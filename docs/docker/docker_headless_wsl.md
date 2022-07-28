# Docker in WSL2 without docker desktop

## motivation and overview

Use docker headless instead of podman to be able to use k3d.

Steps:

- WSL has no systemd -> Solution: Start daemon in .bashrc
- WSL uses newer version of ipttables which is not supported by docker to the given time -> Solution: use update-alternatives to set legacy version

My system:
- WSL2
- Distribution: Debian GNU/Linux 11 (bullseye)
- Kernel: 4.19.104-microsoft-standard

## prerequisites:

- deinstall docker desktop
- restart wsl:  `wsl.exe --shutdown`

## installation

```shell
# INSTALL DOCKER ON DEBIAN
## source: https://docs.docker.com/engine/install/debian/

## install packages to allow apt to use a repository over HTTP
sudo apt update
sudo apt install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release -y

## add Docker’s official GPG key:
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

## set up the stable docker repository
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

## install the latest version of Docker Engine and containerd
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io -y

## add ur user to docker group (so no sudo needed)
sudo usermod -a -G docker $USER

# TEST DOCKER 
docker run hello-world

## if it does not work check the error message of the docker daemon
sudo dockerd
## ctrl + c

## if the reason has something to do with iptables this might be the solution
## https://patrickwu.space/2021/03/09/wsl-solution-to-native-docker-daemon-not-starting/
sudo update-alternatives --config iptables
# choose legay: /usr/sbin/iptables-legacy

## restart wsl. In Powershell run:
# wsl.exe --shutdown

# START DOCKER AT STARTUP OF WSL (THERE IS NO SYSTEMD)
## source: https://blog.nillsf.com/index.php/2020/06/29/how-to-automatically-start-the-docker-daemon-on-wsl2/

## start dockerd without being prompted for a password every time you start a terminal 
sudo cp /etc/sudoers /etc/sudoers.backup
echo "$USER ALL=(ALL) NOPASSWD: /usr/bin/dockerd" | sudo tee --append /etc/sudoers
sudo cat /etc/sudoers

## automatically run docker daemon
echo "" >> ~/.bashrc
echo '# Start Docker daemon automatically when logging in if not running.' >> ~/.bashrc
echo 'RUNNING=`ps aux | grep dockerd | grep -v grep`' >> ~/.bashrc
echo 'if [ -z "$RUNNING" ]; then' >> ~/.bashrc
echo '    sudo dockerd > /dev/null 2>&1 &' >> ~/.bashrc
echo '    disown' >> ~/.bashrc
echo 'fi' >> ~/.bashrc
```

```powershell
# restart wsl
wsl.exe --shutdown
```
