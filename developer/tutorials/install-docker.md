# Install docker and docker-compose

### 1. Create setup.sh

vim setup.sh, and insert below content

```bash
#!/bin/bash
export LC_ALL=C
sudo apt-get update -y
sudo apt install apt-transport-https ca-certificates curl software-properties-common jq net-tools  wget liblz4-tool aria2 -y
#sudo apt-get upgrade -y
# install docker-engine
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt-get update
sudo apt-get install -y docker-ce
echo "Docker installed..."
sudo usermod -aG docker ${whoami}
sudo systemctl enable docker
sudo systemctl start docker
echo "########################################"
echo "##################### install docker-compose ########################"
sudo curl -L https://github.com/docker/compose/releases/download/v2.20.2/docker-compose-linux-x86_64 -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
docker-compose --version
echo "docker-compose installed..."
echo "########################################"
```

### 2. Run setup file

```bash
sh setup.sh
```

### 3. Verify version

```bash
docker version
docker-compose version
```
