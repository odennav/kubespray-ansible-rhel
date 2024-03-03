# ![k8s](https://github.com/odennav/terraform-k8s-aws_ec2/blob/main/icons-k8s-color/icons8-kubernetes-96.png)  Deploy Kubernetes Cluster to CentOS Servers   

This project automates the deployment of a kubernetes cluster using kubespray in centOS 8 and centOS 9 servers.
Vagrant is used to provision VMs.

## Special Credits

Special thanks to [Kubernetes-sigs](https://https://github.com/kubernetes-sigs) for their amazing work.


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

Hardware:
These limits are safeguarded by Kubespray. Actual requirements for your workload can differ. For a sizing guide go to the [Building Large Clusters](https://kubernetes.io/docs/setup/cluster-large/#size-of-master-and-master-components) guide.

- Master
  - Memory: 1500 MB
- Node
  - Memory: 1024 MB


## Getting Started
1. *Provision vagrant vms**
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

4. **Change directory to /**
   ```bash
   cd /
   ```

5. **Update yum package manager**
   ```bash
   yum update
   ```

6. **Install git**
   ```bash
   yum install git
   ```

7. **Clone this repo to / directory in control node**
   ```bash
   git clone git@github.com:odennav/kubespray-centOS.git
   ```

8. **Clone kubernetes-sigs kubespray repo to / directory in control node**
   ```bash
   git clone git@github.com:kubernetes-sigs/kubespray.git
   ```

9. **Run dependencies-install.sh in control node to install necessary dependencies**
   Updating Yum, installing necessary dependencies, and ensuring Python compatibility.
    ```bash
    chmod 770 dependencies-install
    bash dependencies-install
    ```
   

10. **Run kubespray-deploy.sh to deploy Kubernetes to node1, node2 and node3**
   ```bash
   chmod 770 kubespray-deploy.sh
   bash kubespray-deploy.sh
   ```
   It creates a virtual environment, copies SSH keys, updates Ansible inventory, edits host inventory, installs kubectl and deploys Kubernetes cluster.
   Python script  builds inventory.
   Ansible playbook also used to deploy kubernetes cluster from control node to other nodes.


## Reset Kubernetes Cluster
To tear down the infrastructure created by vagrant.
Run this command in kubespray directory
  ```bash
  ansible-playbook -i /inventory/mycluster/hosts.yaml --become --become-user=root  reset.yml
  ```
## Destroying Resources(Optional)
To tear down the VMs created by vagrant.
  ```bash
  vagrant destroy
  ```


Enjoy!
