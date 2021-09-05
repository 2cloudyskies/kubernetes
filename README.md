## Kubernetes
Using Ansible to bring up a K8S cluster running on Ubuntu 18.04 or 20.04. Tested to work on Ubuntu 20.04 successfully. Note that containerd is used instead of Docker. 

Pre-requisites:
- 1 Ubuntu host with ansible installed, this is where the ansible playbooks will be executed. This host must has password-less root SSH access to the K8S nodes. See this [article](https://linuxconfig.org/allow-ssh-root-login-on-ubuntu-20-04-focal-fossa-linux) on how to enable root SSH on Ubuntu, and this [article](https://linuxize.com/post/how-to-enable-ssh-on-ubuntu-20-04/) on how to enable SSH for Ubuntu
- Minimum 1 Ubuntu master, 1 Ubuntu worker nodes running Ubuntu 18.04 or 20.04 and have IP connectivity to each other
- Playbooks have been modified assuming root SSH access to the K8S nodes is available from the Ubuntu host executing the playbooks

##### Note
There seems to be be some problem on the master.yml playbook in printing out the kubeadm join output post kubeadm init. As a workaround, after executing master.yml playbook, ssh to the K8S master node and run the following

kubeadm token create --print-join-command

Original description of installation process is [here](https://buildvirtual.net/deploy-a-kubernetes-cluster-using-ansible/)
