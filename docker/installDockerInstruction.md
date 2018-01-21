## Docker setup

### install docker CE: community docker
* https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/#install-docker-ce

### run hello world
* $ sudo docker run hello-world

### Post-installation steps for Linux
* https://docs.docker.com/engine/installation/linux/linux-postinstall/

### Add user to docker group to avoid sudo
* $ sudo usermod -aG docker $(whoami)

### log out and log back in so that your group membership is re-evaluated
* $ docker run hello-world


### Enable docker at startup 
* $ sudo systemctl enable docker

### Install nvidia-docker 
* $ https://github.com/NVIDIA/nvidia-docker

### Test nvidia-smi with the latest official CUDA image
* $ docker run --runtime=nvidia --rm nvidia/cuda nvidia-smi

