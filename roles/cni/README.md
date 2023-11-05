# Container network interface
This Ansible role installs Container Runtime Interfaces (CNI) in a Kubernetes cluster.   

## Main steps to execute this Ansible playbook :
1. Possibly adjust variables in roles/cni/var./main.yml (check comments in the file to understand effect of respective variables)
   
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
│   ├── calico_crd.j2
│   └── calico_IPPools.j2
├── tests
│   ├── inventory
│   └── test.yml
└── vars
    └── main.yml
```
# TODO
* Full tests with Kubernetes nodes running Ubuntu 20.04 LTS or higher

