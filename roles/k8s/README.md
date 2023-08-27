# Kubernetes master and worker nodes 
This Ansible role installs master node(s) and N worker nodes 
Note that the playbook detects if target OS is Ubuntu or Rocky-Linux/Centos/Redhat and runs respective tasks.
So there are variables specific to each OS.

Fully tested using :
* Ansible 2.9.6 on Ubuntu 20.04 LTS
* Rocky-linux 8 as Kubernetes cluster nodes (should work on CentOS 8 and RedHat 8 as well)
* Kubernetes 1.23 as container orchestrator

## Main steps to execute this Ansible playbook :
1. Possibly adjust variables in role/k8s/var/main.yml (check comments in the file to understand effect of respective variables)
   
## Directory structure :
```
.
├── defaults
│   └── main.yml
├── files
├── handlers
│   └── main.yml
├── meta
│   └── main.yml
├── README.md
├── tasks
│   ├── main.yml
│   ├── rocky-linux_master.yml
│   ├── rocky-linux_workers.yml
│   ├── ubuntu_master.yml
│   └── ubuntu_workers.yml
├── templates
│   ├── cluster-kubelet-config.j2
│   └── kubelet.j2
├── tests
│   ├── inventory
│   └── test.yml
└── vars
    └── main.yml
```
# TODO
* Full tests with Kubernetes on nodes running Ubuntu 20.04 LTS or higher

