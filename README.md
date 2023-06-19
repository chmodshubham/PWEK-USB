# PWEK Deployment

Deployment of ESP(Edge Software Provisioner) using USB Drive Method.

## Pre-requisite

 - **ESP**: Minimum Recommended Hardware or VM with `2 CPUs`, `20GB HD`, and `2GB RAM`, running any Linux Distro (headless recommended) that supports Docker.
 - `docker` 18.09.3 or greater
 - `docker-compose` v1.23.2 or greater
 - `bash` v4.3.48 or greater

## Pre-Requisite Installation:

#### docker

```bash
# update packages
sudo apt-get update
sudo apt-get upgrade

sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release

sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

#### docker-composes

```bash
# standalone docker-compose installation
sudo curl -SL https://github.com/docker/compose/releases/download/v2.18.1/docker-compose-linux-x86_64 -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose

# to verify the installation
docker compose version
```

If `docker-compose` is not installed correctly, then use this(*least recommended*):

```bash
sudo apt update
sudo apt install docker-compose
```

#### git

```bash
sudo apt install git
```

#### Python 3.6 with PyYAML

```bash
sudo apt update
sudo apt install -y python3 python3-pip
pip3 install PyYAML fqdn
```

#### Others

```bash
sudo apt install net-tools

# for searching 
sudo apt install silversearcher-ag

# to get the progress output of flashing the USB drive
sudo apt install pv 
```

## Deployment

A directory is missing, `/opt/pwek_offline_files/` which is used during pwek-deployment. (**skip it for now**)

```bash
sudo su -
git clone https://github.com/ShubhamKumar89/PWEK.git --recursive --branch=main ~/pwek
cd ~/pwek
./pwek_aio_provision.py --init-config > prov.yml
```

Copy the changes from the `prov.yml` in this repository to the `prov.yml` in the pwek directory. And also change the mac addreess inside the file `prov.yml` according to your mac address.

```bash
./pwek_aio_provision.py --config prov.yml --run-esp-for-usb-boot
```

## Flash the Installation Image

Change the `sdb` with your flash drive. To check all the attached USBs, use `lsblk`.

```bash
./pwek_flash.sh efi ./out/Smart_Edge_Open_Private_Wireless_Experience_Kit-efi.img --dev /dev/sdb1
```
