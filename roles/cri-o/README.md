# CRI-O containers runtime interface 
This Ansible role installs CRI-O containers runtime  
Note that the playbook detects if target OS is Ubuntu or Rocky-Linux/Centos/Redhat and runs respective tasks.   
So there are variables specific to each OS.   

Fully tested using :
* Ansible 2.9.6 on Ubuntu 20.04 LTS
* Rocky-linux 8 as Kubernetes cluster nodes (should work on CentOS 8 and RedHat 8 as well)
* CRI-O 1.23 as container runtime

## Main steps to execute this Ansible playbook :
1. Possibly adjust variables in role/cri-o/var/main.yml
   
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
│   ├── rocky-linux.yml
│   └── ubuntu.yml
├── templates
│   ├── 100-crio-bridge.conf.j2
│   ├── crio.conf.j2
│   └── registries.conf.j2
├── tests
│   ├── inventory
│   └── test.yml
└── vars
    └── main.yml
```
# TODO
* Full tests with Kubernetes nodes running Ubuntu 20.04 LTS or higher

