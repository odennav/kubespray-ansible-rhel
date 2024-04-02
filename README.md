## ![k8s](https://github.com/odennav/terraform-k8s-aws_ec2/blob/main/icons-k8s-color/icons8-kubernetes-48.png)  Deployment of a Kubernetes Cluster with Kubespray using Bash Scripts   

This project automates the deployment of a kubernetes cluster with kubespray in CentOS VMs.

Ansible scripts used to automatically setup a kubernetes cluster.

Vagrant is used to provision VMs.


## Requirements

- **Minimum required version of Kubernetes is v1.27**
- **Ansible v2.14+, Jinja 2.11+ and python-netaddr is installed on the machine that will run Ansible commands**
- The target servers must have **access to the Internet** in order to pull docker images. Otherwise, additional configuration is required.
- The target servers are configured to allow **IPv4 forwarding**.
- If using IPv6 for pods and services, the target servers are configured to allow **IPv6 forwarding**.
- The **firewalls are not managed**, you'll need to implement your own rules the way you used to.
    in order to avoid any issue during deployment you should disable your firewall.
- If kubespray is run from non-root user account, correct privilege escalation method
    should be configured in the target servers. Then the ansible_become flag
    or command parameters --become or -b should be specified.
- Get [Chocolatey](https://chocolatey.org/install) and use it to install vagrant:
  ```bash
  choco install vagrant
  ```

## Getting Started
1. **Provision vagrant VMs**
   ```bash
   vagrant up
   ```

2. **Login to control vm**
   ```bash
   vagrant ssh control
   ```

3. **Change password of root user**
   This new password will be required when public RSA keys are being transferred to kube nodes.
   ```bash
   sudo passwd
   ```

4. **Update yum package manager**
   ```bash
   cd /
   yum update
   ```

5. **Install git**
   ```bash
   yum install git
   ```

6. **Clone this repo to / directory in control node**
   ```bash
   git clone git@github.com:odennav/kubespray-bash-ansible.git
   ```

7. **Clone kubernetes-sigs kubespray repo to / directory in control node**
   ```bash
   git clone git@github.com:kubernetes-sigs/kubespray.git
   ```

8. **Run dependencies-install.sh in control node to install necessary dependencies**

   Updating Yum, installing necessary dependencies, and ensuring Python compatibility.
   ```bash
   chmod 770 dependencies-install
   ./dependencies-install
   ```
   

9. **Setup system for Ansible playbook execution**

    This bash script copies SSH keys, updates Ansible inventory, builds host inventory manifest and installs kubectl.
    ```bash
    chmod 770 k8s-env-build.sh
    ./k8s-env-build.sh
    ```


   
10. **Run Ansible playbook to to deploy kubernetes cluster**
    
    Change directory to your local kubespray repo and execute cluster playbook
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
## Destroying Resources(Optional)
To destroy the VMs created by vagrant.
  ```bash
  vagrant destroy
  ```

## Special Credits

Special thanks to [Kubernetes-sigs](https://https://github.com/kubernetes-sigs) for their amazing work.


Enjoy!
