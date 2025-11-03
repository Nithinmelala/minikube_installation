# Minikube & OpenStack on VirtualBox (Ubuntu 20.04)

A clean, reproducible guide to install **VirtualBox**, **kubectl**, and **Minikube** on Ubuntu 20.04, then deploy **OpenStack** via **DevStack** inside a VirtualBox VM.  
_All shell snippets are `bash` with a leading `$` prompt, and each block starts with a `##` header comment you can keep as inline documentation when running the commands._

---

## Table of Contents
1. [Prerequisites](#prerequisites)
2. [Install Required Packages (Host)](#install-required-packages-host)
3. [Install VirtualBox (Host)](#install-virtualbox-host)
4. [Enable Nested Virtualization](#enable-nested-virtualization)
5. [Install kubectl (Host)](#install-kubectl-host)
6. [Install Minikube (Host)](#install-minikube-host)
7. [Start & Verify Minikube](#start--verify-minikube)
8. [OpenStack via DevStack (Inside a VirtualBox VM)](#openstack-via-devstack-inside-a-virtualbox-vm)
9. [Edit `local.conf` (Example)](#edit-localconf-example)
10. [Post-Deployment Checks](#post-deployment-checks)
11. [Troubleshooting](#troubleshooting)
12. [Cleanup](#cleanup)
13. [Notes & Best Practices](#notes--best-practices)

---

## Prerequisites

- **Host OS:** Ubuntu 20.04 LTS  
- **CPU:** Intel VT-x / AMD-V enabled in BIOS/UEFI  
- **RAM:** ≥ 8 GB (16 GB recommended if running Minikube and an OpenStack VM together)  
- **Disk:** ≥ 40 GB free  
- **Network:** Internet access  
- **Access:** `sudo` privileges on host and VM

---

## Install Required Packages (Host)

```bash
## Install Required Packages (Host)
$ sudo apt-get update && sudo apt-get install -y apt-transport-https curl


## Install VirtualBox Hypervisor (Host)
$ sudo apt-get install -y virtualbox virtualbox-ext-pack

![image](https://user-images.githubusercontent.com/48765431/123541247-1302de80-d776-11eb-8628-3494cbf1b613.png)
![image](https://user-images.githubusercontent.com/48765431/123541269-362d8e00-d776-11eb-886e-9f3ca6ccc6b6.png)


## Reboot & Enable Nested Virtualization (Host → VM Settings)

Important: After reboot, open VirtualBox → select the VM you will use for OpenStack → Settings → System → Processor → enable “Enable Nested VT-x/AMD-V”.

$ sudo reboot
```
![image](https://user-images.githubusercontent.com/48765431/123541019-b9e67b00-d774-11eb-8dbb-9bc0dafdf23e.png)



## Install kubectl (Host)
## Download latest stable kubectl
$ curl -LO "https://dl.k8s.io/release/$(curl -Ls https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
## Make executable and move into PATH
$ chmod +x kubectl && sudo mv kubectl /usr/bin/
## Verify kubectl installation
$ which kubectl
$ kubectl version --client

```
![image](https://user-images.githubusercontent.com/48765431/123540610-80ad0b80-d772-11eb-897e-417d4999c5b2.png)
![image](https://user-images.githubusercontent.com/48765431/123540640-a3d7bb00-d772-11eb-9843-31ee779bea26.png)


## Install Minikube (Host)
## Download latest Minikube
$ curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
## Make executable and move into PATH
$ chmod +x minikube && sudo mv minikube /usr/local/bin/
## Verify Minikube
$ minikube version

![image](https://user-images.githubusercontent.com/48765431/123541098-28c3d400-d775-11eb-9079-c6dbb14871e6.png)

## Start Minikube (VirtualBox driver)
$ minikube start --driver=virtualbox

```
![image](https://user-images.githubusercontent.com/48765431/123540685-daadd100-d772-11eb-9855-73163469950b.png)


## Verify Minikube status and node readiness
$ minikube status
$ kubectl get nodes

![image](https://user-images.githubusercontent.com/48765431/123541178-b7385580-d775-11eb-8598-b528bf5b08a8.png)



## OpenStack via DevStack (Inside a VirtualBox VM)

Perform the following steps inside an Ubuntu 20.04 guest VM created in VirtualBox.
Ensure this VM also has Nested VT-x/AMD-V enabled (see above).


## Update the VM (Guest)
$ sudo apt-get update
$ sudo apt-get upgrade -y

## Create 'stack' user (passwordless sudo) and switch to it
$ sudo useradd -s /bin/bash -d /opt/stack -m stack
$ echo "stack ALL=(ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/stack
$ sudo su - stack

## Install Git & clone DevStack (updated repo)
$ sudo apt-get install -y git
$ git clone https://opendev.org/openstack/devstack
$ cd devstack/

## Copy sample local.conf into place
$ cd samples/
$ cp local.conf ../
$ cd ..

### Modify the local.conf file 
![image](https://user-images.githubusercontent.com/48765431/125175174-e5e60f80-e1fc-11eb-8156-32e30c61c6c2.png)
### Run the stack file 
```
## Run DevStack deployment
$ ./stack.sh

## Load admin OpenStack credentials and run sanity checks
$ source /opt/stack/devstack/openrc admin admin
$ openstack project list
$ openstack compute service list
$ openstack network agent list

## If stack.sh fails, inspect logs
$ sudo tail -n +1 /opt/stack/logs/stack.sh.log

## Clean and retry DevStack
$ cd ~/devstack
$ ./unstack.sh
$ ./clean.sh
$ ./stack.sh


## Tear down DevStack (inside VM)
$ cd ~/devstack
$ ./unstack.sh
$ ./clean.sh

## Remove Minikube (host)
$ minikube delete

