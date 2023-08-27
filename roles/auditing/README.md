# Kubernetes auditing 
This Ansible role active Kubernetes auditin active Kubernetes auditing.  
Very usefull to diagnose RBAC issues.

Fully tested using :
* Ansible 2.10.8 on Ubuntu 22.04 LTS
* Rocky-linux 8 as Kubernetes cluster nodes (should work on CentOS 8 and RedHat 8 as well)
* Kuberetes 1.27 as container orchestrator

## Main steps to execute this Ansible playbook :
1. Possibly adjust variables in role/auditing/var/main.yml (check comments in the file to understand effect of respective variables)
   
## Directory structure :
```
.
├── defaults
│   └── main.yml
├── handlers
│   └── main.yml
├── meta
│   └── main.yml
├── README.md
├── tasks
│   └── main.yml
├── templates
│   └── K8S_Audit-Policy_simple.j2
├── tests
│   ├── inventory
│   └── test.yml
└── vars
    └── main.yml
```
# TODO
* Full tests on nodes running Ubuntu 20.04 LTS or higher

