# Create-K8s-Cluster-on-Virtualbox
This Guide will help to understand the steps/process to create Kubernetes cluster locally on VirtualBox using Kubeadm.
This example assume that everyting is being done on Ubuntu, But the steps are same for all Linux System.

## First VM

1. Create a VM in Virtual box. 
2. Attach it to two Networks. Click on Setting -->  Network --> Adapter and -
   - Enable Bridge Adapter and select one from Dropdown. so that VM can connect to Internet.
   - Enable Other Adaptor and attach to HOST-ONLY Adaptor and select network from dropdown (Ethernet Adepter). This network is to create a network locally between all the VM within VirtualBox.
    
3. Start VM and run following commands on terminal -
    - sudo apt get update
    - sudo apt get upgrade
    - sudo apt install openssh-server
    - sudo apt install curl
    - sudo swapoff -a
    
4. update the file ---> sudo nano /etc/fstab  and comment the line which have swap in it.

5. Note IP address of HOST-ONLY network using command "ip addr" in terminal.
   - Now make following entry in file sudo nano /etc/network/interfaces . we are doing this step to make IP Address static, so that it does not change after reboot.
      - #configure enp0s8
      - auto enp0s8
      - iface enp0s8 inet static
      - address _ip-address-of-enp0s8-network         got from ip addr_
      - netmask 255.255.255.0

5. Install Container Runtime (Docker/podman....).
    - _K8s uses "systemd" cgroups driver but docker ues "cgroupfs". So this is mismatch between cgroups. If you plan to use Docker, you have to change the cgroups of docker to use systemd as K8s is using otherwise kubelet will not start and will exist with error code 1._ 
   
   - To change cgroups create/update the file -->  sudo nano /etc/docker/daemon.json  with following lines - 
     - {
  "exec-opts": ["native.cgroupdriver=systemd"]
   }
   
   - and then restart the docker  --->  sudo systemctl restart docker

6. Install Container Runtime Interfaces (containerd /CRI-O /Docker Engine/ Mirantis Container Runtime). 
   If you follow the steps to install docker engine you may install containerd via sudo apt install.
   
7. Install Kubeadm
8. Install Kubelet
9. Install Kubectl



We have to do all the above steps on all VM we are planning to join K8s Cluster. So best is to do all steps on one VM and then clone it. It will save time and space.


## Clone VM's
10. Now the Above created VM is ready to get cloned. Follow below steps in Virtualbox -
   - Select VM you want to clone   --->  Machine  ---> Clone  --->  Give a name to VM and Under "MAC Address Policy"  choose "Generate new MAC Address for all network          adapter"   --->  Linked Clone (to save space otherwise you are free to chosse Full clone)  ---> Clone.
   
 11. Now next step is to change Hostname and set static IP Address on all the VM's. Static IP (which does not change when machine reboot) address is mandatory in kubernetes and assign name to each host. IP and hostname should be different for each VM.
 - open file  -->  sudo nano /etc/hostname    and change hostname
 - open file  --> sudo nano /etc/hosts        and change hostname
 - sudo nano /etc/network/interfaces    , comment enp0s8 network in this file, Reboot VM, check new IP for enp0s8 and update new IP in this file and uncomment it. 
   
 
**NOTE:-**  During Initializing Master node may need IP Address for Pod network (It will depends on Network you select for Pod, Calico Passes IP ). Please follow the [Kubernetes Documentaion](https://kubernetes.io/docs/home/) Page to know How to insatll Kubelet, kubeadm, kubectl, Initialize Master node and join worker node.
