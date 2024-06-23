## ![k8s](https://github.com/odennav/terraform-k8s-aws_ec2/blob/main/docs/icons8-kubernetes-48.png)  Deployment of a Kubernetes Cluster with Kubespray   

This project automates the deployment of a kubernetes cluster with kubespray in Test environment.

Ansible and Bash scripts are used to setup a kubernetes cluster.
Vagrant is utilized to provision VMs.


## Requirements

- Minimum required version of Kubernetes is **`v1.27`**

- **`Ansible v2.14+`**, **`Jinja 2.11+`** and **`python-netaddr`** is installed on the machine that will run Ansible commands.

- The target servers must have access to the Internet in order to pull docker images. Otherwise, additional configuration is required.

Get [Chocolatey](https://chocolatey.org/install) and use it to install vagrant:

```bash
  choco install vagrant
  ```

## Getting Started

Provision vagrant VMs
```bash
vagrant up
```

Login to `control` VM
```bash
vagrant ssh control
```

Change password of `root` user

This new password will be required when public RSA keys are being transferred to kube nodes.
```bash
sudo passwd
```

Update yum package manager
```bash
cd /
yum update
```

Install Git
```bash
yum install git
```

Clone this repo to `/` directory in control node
```bash
git clone git@github.com:odennav/kubespray-bash-ansible.git
```

Clone kubernetes-sigs kubespray repo to `/` directory in control node
```bash
git clone git@github.com:kubernetes-sigs/kubespray.git
```

Run `dependencies-install.sh` script in `control` node to install necessary dependencies, update yum package index and ensuring python version compatibility.

```bash
chmod 770 dependencies-install
./dependencies-install
```   

Setup system for Ansible playbook execution

This bash script copies SSH keys, updates Ansible inventory, builds host inventory manifest and installs kubectl.
```bash
chmod 770 k8s-env-build.sh
./k8s-env-build.sh
```
   
Run Ansible playbook to to deploy kubernetes cluster
    
Change directory to your local kubespray repository and execute `cluster.yml` ansible playbook
```bash
cd /kubespray
ansible-playbook -i inventory/mycluster/hosts.yaml --become --become-user=root cluster.yml   
```

## Reset Kubernetes Cluster

To remove current kubernetes cluster, run playbook as root user.

Run this command in kubespray directory
```bash
cd /kubespray
ansible-playbook -i inventory/mycluster/hosts.yaml --become --become-user=root  reset.yml
```
### Destroying Resources(Optional)

To destroy the VMs created by vagrant.
```bash
vagrant destroy
```

## Special Credits

Special thanks to [Kubernetes-sigs](https://https://github.com/kubernetes-sigs) for their amazing work.


Enjoy!
