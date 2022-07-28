# Create-K8s-Cluster-on-Virtualbox
This Guide will help to understand the steps/process to create Kubernetes cluster locally on VirtualBox using Kubeadm.
This example assume that everyting is being done on Ubuntu, But the steps are same for all Linux System.

1. Create a VM in Virtual box. 
2. Attach it to two Networks. Click on Setting -->  Network --> click on Adapter and -
    A) Enable Bridge Adapter and select one from Dropdown. so that it can connect to Internet.
    B) Enable other adaptor and attach to HOST-ONLY Adaptor and select network from dropdown (Ethernet Adepter). This is to create a network between all the VM within              VirtualBox.
    
3. Start it and run following commands -
    sudo apt get update
    sudo apt get upgrade
    sudo apt install openssh-server
    sudo apt install curl
    sudo swapoff -a
    
4. update the file ---> sudo nano /etc/fstab  and comment the line which have swap in it.

5. Note IP address of HOST-ONLY network using command "ip addr" in terminal

5. Install Container Runtime (Docker/podman....)
   K8s uses "systemd" cgroups driver but docker ues "cgroupfs". So this is mismatch between cgroups. If you plan to use Docker, you have to change the cgroups of docker to    use systemd as K8s Is using otherwise kubelet will not start and will exist with error code 1. 
   
   To change cgroups create/update the file -->  sudo nano /etc/docker/daemon.json  with following lines - 
   {
  "exec-opts": ["native.cgroupdriver=systemd"]
   }
   
   and then restart the docker  -  sudo systemctl restart docker

6. Install Container Runtime Interfaces (containerd /CRI-O /Docker Engine/ Mirantis Container Runtime). 
   If you follow the steps to install docker engine you may install containerd via sudo apt install.
   
7. Install Kubeadm
8. Install Kubelet
9. Install Kubectl



We have to do all the above steps on all VM we are planning to join K8s Cluster. So best is to do all steps on one VM and then clone it.


10. Now the Above created VM is ready to get cloned. Follow below steps in Virtualbox -
   Select VM you want to clone   --->  Machine  ---> Clone  --->  Give a name to VM and Under "MAC Address Policy"  choose "Generate new MAC Address for all network          adapter"      --->  Linked Clone (to save space otherwise you are free to chosse Full clone)  ---> Clone.
   
 11. 
   
