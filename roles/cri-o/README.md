# CRI-O containers runtime interface 
This Ansible role installs CRI-O containers runtime  

## Main steps to execute this Ansible playbook :
1. Possibly adjust variables in roles/cri-o/vars/main.yml (check comments in the file to understand effect of respective variables)
   
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

