Ansible Play Book To Create Kubernetes Cluster

VM Requirements: 

No of Master Nodes : 3 (2CPUs, 2GB RAM and running centos7)
No of Worker Nodes : 2 (1CPUs, 1GB RAM and running centos7)
No of LoadBalancer Node : 1 (1CPUs, 1GB and RAM running centos7)

VMs are cofigured using Vagrant 

Vagrant Code for creating the VMS as per the above is available here : https://github.com/connectbaseer/KubernetesHACluster-Vagrant.git 

IF you have configured the VMs using my vagrant project mentioned above without changing any details then you dont need to change anything. you can just enable password less ssh from your Ansible machine to the Kubernetes cluster VMs.

Note: before running the playbook create empty folder named joinFiles in the 

To tun the playbook : 

```
ansible-playbook k8s-config-playbook.yaml
```

If you have not setup the VMs using the above then you ned to make the changes accordingly in below files

File 1 : 

KubernetesHACluster-Ansible/group_vars/k8scluster

Change the ip Addresses Accordingly: 

```
k8smaster1: 192.168.30.5
k8smaster2: 192.168.30.6
k8smaster3: 192.168.30.7
k8sworker1: 192.168.30.10
k8sworker2: 192.168.30.11
```
File 2 : 

KubernetesHACluster-Ansible/group_vars/k8slb 

Change the ip Addresses Accordingly: 

```
---
ansible_user: vagrant
ansible_become: true
k8smaster1: 192.168.30.5
k8smaster2: 192.168.30.6
k8smaster3: 192.168.30.7
...
```

File 3 

KubernetesHACluster-Ansible/hosts

change the hostnames of the Kubernetes Cluster VMs

```
[control]
mydevmachine # Ansible VM hostname 

[k8slb]
k8s-lb    # K8s LB hostname

[k8smaster]
k8s-master-01  # K8s Master 01 hostname
k8s-master-02  # K8s Master 02 hostname
k8s-master-03  # K8s Master 03 hostname

[k8sworker]
k8s-worker-01  # K8s worker 01 hostname
k8s-worker-02  # K8s worker 02 hostname

[k8scluster:children]
k8smaster
k8sworker
```