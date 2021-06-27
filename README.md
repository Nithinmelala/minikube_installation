# Minikube_on_Virtualbox(Ubuntu-20.04)

### Install Required Packages
``` 
$ sudo apt-get update && sudo apt-get install -y apt-transport-https && sudo apt install -y curl 
```
### Install VirtualBox Hypervisor
``` 
$ sudo apt-get install -y virtualbox virtualbox-ext-pack
```
![image](https://user-images.githubusercontent.com/48765431/123541247-1302de80-d776-11eb-8628-3494cbf1b613.png)
![image](https://user-images.githubusercontent.com/48765431/123541269-362d8e00-d776-11eb-886e-9f3ca6ccc6b6.png)
### Reboot the system and check the Enable Nested VT-x/AMD-V option in virtual box settings
```
$ reboot
```
![image](https://user-images.githubusercontent.com/48765431/123541019-b9e67b00-d774-11eb-8dbb-9bc0dafdf23e.png)
### Install Kubectl 
```
$ curl -Lo kubectl https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
$ chmod +x kubectl && sudo mv kubectl /usr/bin/.
$ which kubectl
$ kubectl version
```
![image](https://user-images.githubusercontent.com/48765431/123540610-80ad0b80-d772-11eb-897e-417d4999c5b2.png)
![image](https://user-images.githubusercontent.com/48765431/123540640-a3d7bb00-d772-11eb-9843-31ee779bea26.png)

### Install Minikube
```
$ curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 
$ chmod +x minikube && sudo mv minikube /usr/local/bin/
$ minikube version
```
![image](https://user-images.githubusercontent.com/48765431/123541098-28c3d400-d775-11eb-9079-c6dbb14871e6.png)

### Start Minikube
``` 
$ minikube start 
```
![image](https://user-images.githubusercontent.com/48765431/123540685-daadd100-d772-11eb-9855-73163469950b.png)

### Check the status 
``` 
$ minikube status 
```
![image](https://user-images.githubusercontent.com/48765431/123541178-b7385580-d775-11eb-8598-b528bf5b08a8.png)
