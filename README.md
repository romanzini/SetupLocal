# Guia rápido de instalação do ambiente com WSL2 + GIT + DOCKER + KUBERNETES Local

## 1. Instalação do WSL 2
``` bash
wsl --update
```

### Instale o Ubuntu 22.04
```bash
wsl --install Ubuntu-22.04
```

### Atualize o Ubuntu 22.04
```bash
sudo apt update && sudo apt upgrade 
```

### Crie um arquivo chamado `.wslconfig` na raiz da sua pasta de usuário `(C:\Users\<seu_usuario>)` e defina estas configurações:
```txt
[wsl2]
memory=16GB
processors=8
swap=4GB
```

## 2. Configure o GIT 
### 1. Verificando a configuração atual do Git 
```bash
git config --list 
```
### 2. Configurando o nome do usuário 
```bash
git config --global user.name "Rodrigo Romanzini" 
```
### 3. Configurando o email do usuário  
```bash
git config --global user.email "rodrigo.romanzini@gmail.com" 
```
### 4. Configurando o editor de texto padrão para o Git (pode ser qualquer editor)   
```bash
git config --global core.editor vim
```
### 5. Configurando a branch padrão como "main"    
```bash
git config --global init.defaultBranch main 
```
### 6. Gerando chave SSH na máquina local (unix)    
```bash
ssh-keygen -t rsa -b 2048 
```
### 7. Copiar a chave pública gerada para o GIT
#### Em Settings/SSH and GPG Key -> New SSH key 


## 3. Instale Docker Engine (Docker Nativo) diretamente instalado no WSL2
https://docs.docker.com/engine/install/

### 1. Set up Docker's apt repository
```bash
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update 
```

### 2. Install the Docker packages
```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

### 3. Verify that the installation is successful by running the hello-world image:
```bash
sudo docker run hello-world
```

### PostInstall

### 1. Create the docker group
```bash
sudo groupadd docker
```

### 2. Add your user to the docker group.
```bash
sudo usermod -aG docker $USER
```

### 3. Log out and log back in so that your group membership is re-evaluated.
```bash
newgrp docker
```

### 4. Verify that you can run docker commands without sudo.
```bash
docker run hello-world
```

## 4. Instale KUBECTL via native package management
https://kubernetes.io/docs/tasks/tools/

### 1. Update the apt package index and install packages needed to use the Kubernetes apt repository:
```bash
sudo apt-get update
# apt-transport-https may be a dummy package; if so, you can skip that package
sudo apt-get install -y apt-transport-https ca-certificates curl gnupgs
```
### 2. Download the public signing key for the Kubernetes package repositories:
```bash
# If the folder `/etc/apt/keyrings` does not exist, it should be created before the curl command, read the note below.
# sudo mkdir -p -m 755 /etc/apt/keyrings
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.31/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
sudo chmod 644 /etc/apt/keyrings/kubernetes-apt-keyring.gpg # allow unprivileged APT programs to read this keyring
```
### 3. Add the appropriate Kubernetes apt repository. If you want to use Kubernetes version different than v1.31, replace v1.31 with the desired minor version in the command below:
```bash
# This overwrites any existing configuration in /etc/apt/sources.list.d/kubernetes.list
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.31/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
sudo chmod 644 /etc/apt/sources.list.d/kubernetes.list   # helps tools such as command-not-found to work correctly
```
### 4. Update apt package index, then install kubectl:
```bash
sudo apt-get update
sudo apt-get install -y kubectl
kubectl cluster-info
```
