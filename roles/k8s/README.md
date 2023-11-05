# Kubernetes master and worker nodes 
This Ansible role installs master node(s) and N worker nodes 

## Main steps to execute this Ansible playbook :
1. Possibly adjust variables in roles/k8s/vars/main.yml (check comments in the file to understand effect of respective variables)
   
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

