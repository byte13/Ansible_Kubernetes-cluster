# Rancher 
This Ansible role installs cert-manager
Fully tested using :
* Ansible 2.10.8 on Ubuntu 22.04 LTS
* Rocky-linux 9 as Kubernetes cluster nodes (should work on CentOS 9 and RedHat 9 as well)
* Kubernetes 1.30 as destination cluster
* CRI-O 1.30 as container runtime

## Main steps to execute this Ansible playbook :
1. Possibly adjust variables in role/cert-manager/var/main.yml (check comments in the file to understand effect of respective variables)
   
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
│   └── ubuntu_master.yml
├── templates
├── tests
│   ├── inventory
│   └── test.yml
└── vars
    └── main.yml
```
# TODO
* Full tests with Kubernetes on nodes running Ubuntu 20.04 LTS or higher

