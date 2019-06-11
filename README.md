# Kubernetes Ansible Playbook

Build a Kubernetes cluster on Ubunti 18.04(bionic) using Ansibl

# Pre-requisite
- Install latest Oracle Virtualbox
- Install latest vagrant
- Install latest ansible package
 
# Software provisioned in this process
- Ubuntu: 18.04 (bionic)
- docker: latest (at this time it is 18.x)
- kubernetes: latest (at this time it is 1.4.x)
- snap
- helm

# Understanding the files
- Vagrantfile: Vagrant creates the Ubuntu 18.04 VMs. Ansible provisions the VMs
  - Creates 1 master and 2 worker nodes. Update WORKERS to create a different number of worker nodes
  - Master will have IP address: 192.168.50.50
  - Worker nodes will have IP address: 192.168.50.51 and on
- k8s/master.yml: Provision kubernetes master node and start the cluster
- k8s/node.yml: Provision kubernetes worker node and join the cluster

# Prep
- Open Vagrantfile
- Update HOST_USER_ID to match the user id executing this vagrant script

# Create kubernetes cluster
- Run `vagrant up`
- Wait till the playbook is executed
- Login to master: `vagrant ssh k8s-master`
- Run `kubectl get no`. You should see all nodes in Running status:

        vagrant@k8s-master:~$ kubectl get no
        NAME         STATUS   ROLES    AGE     VERSION
        k8s-master   Ready    master   9m19s   v1.14.3
        k8s-node1    Ready    <none>   6m58s   v1.14.3
        k8s-node2    Ready    <none>   2m6s    v1.14.3
- Run `kubectl cluster-info` and you should see:

        vagrant@k8s-master:~$ kubectl cluster-info
        Kubernetes master is running at https://192.168.50.50:6443
        KubeDNS is running at https://192.168.50.50:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
