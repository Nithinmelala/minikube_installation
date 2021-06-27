# Easy_way_to_install_the_Minikube_on_Virtualbox(Ubuntu-20.04)

### Install Required Packages
``` $ sudo apt-get update && sudo apt-get install -y apt-transport-https && sudo apt install -y curl ```
### Install VirtualBox Hypervisor
``` 
$ sudo apt-get install -y virtualbox virtualbox-ext-pack
```
![image](https://user-images.githubusercontent.com/48765431/123540790-5d369080-d773-11eb-8b5c-de5aba68a100.png)
![image](https://user-images.githubusercontent.com/48765431/123540807-6c1d4300-d773-11eb-81bd-d5d9838afbef.png)

### Reboot the system and check the Enable Nested VT-x/AMD-V option in virtual box settings
```
$ reboot
```
![image](https://user-images.githubusercontent.com/48765431/123540865-b0a8de80-d773-11eb-89b2-b0aebde2ef24.png)

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
![image](https://user-images.githubusercontent.com/48765431/123540624-96bacc00-d772-11eb-990f-3cf0fe73e9ca.png)

### Start Minikube
``` 
$ minikube start 
```
![image](https://user-images.githubusercontent.com/48765431/123540685-daadd100-d772-11eb-9855-73163469950b.png)

### Check the status 
``` 
$ minikube status 
```
![image](https://user-images.githubusercontent.com/48765431/123540692-e26d7580-d772-11eb-9cac-1b1667bd065e.png)
