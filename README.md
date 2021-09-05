## Using Ansible to bring up K8S on Ubuntu
Using Ansible to bring up a K8S cluster running on Ubuntu 18.04 or 20.04. Tested to work on Ubuntu 20.04 successfully. Note that containerd is used instead of Docker. 

Pre-requisites:
- 1 Ubuntu host with ansible installed, this is where the ansible playbooks will be executed. This host must has password-less root SSH access to the K8S nodes. See this [article](https://linuxconfig.org/allow-ssh-root-login-on-ubuntu-20-04-focal-fossa-linux) on how to enable root SSH on Ubuntu, and this [article](https://linuxize.com/post/how-to-enable-ssh-on-ubuntu-20-04/) on how to enable SSH for Ubuntu
- Minimum 1 Ubuntu master, 1 Ubuntu worker nodes running Ubuntu 18.04 or 20.04 and have IP connectivity to each other
- Playbooks have been modified assuming root SSH access to the K8S nodes is available from the Ubuntu host executing the playbooks

#### Note
There seems to be be some problem on the master.yml playbook in printing out the <span style="color:blue">kubeadm join</span> output post <span style="color:blue">kubeadm init</span>. As a workaround, after executing master.yml playbook, ssh to the K8S master node and run the following to the the output of <span style="color:blue">kubeadm join</span> for use on the worker node.

<pre><code>kubeadm token create --print-join-command</code></pre>

Alternatively on the Ubuntu host running the playbooks, there should be a <span style="color:blue">/tmp/kubernetes_join_command</span> file that contains the actual <span style="color:blue"> kubeadm join </span> output

Original description of installation process is [here](https://buildvirtual.net/deploy-a-kubernetes-cluster-using-ansible/)

#### Preparing the Ubuntu hosts
Below are some of the steps to prepare the Ubuntu hosts

<pre><code>sudo apt update
sudo apt install net-tools
sudo apt install openssh-server
sudo ufw disable
sudo sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config
sudo systemctl restart ssh

## set root password, by default root user has no password in Ubuntu preventing SSH login
sudo passwd

## install Ansible<span style="color:blue">*</span>
sudo apt update
sudo apt install software-properties-common
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install ansible

## install Git<span style="color:blue">*</span>
sudo apt install git

## generate public/private ssh key pair<span style="color:blue">*</span>
ssh-keygen -t rsa -b 4096 -C "your_email@domain.com"

## copy public key to the remote ansible targets<span style="color:blue">*</span>
ssh-copy-id remote_username@server_ip_address


<span style="color:blue">*-only on jump host</span>
</code></pre>
