# Kubernetes auditing 
This Ansible role active Kubernetes auditin active Kubernetes auditing.  
Very usefull to diagnose RBAC issues.

Fully tested using :
* Ansible 2.9.6 on Ubuntu 20.04 LTS
* Rocky-linux 8 as Kubernetes cluster nodes (should work on CentOS 8 and RedHat 8 as well)
* Kuberetes 1.23 as container orchestrator

## Main steps to execute this Ansible playbook :
1. Possibly adjust variables in role/auditing/var/main.yml
   
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
│   └── B13_K8S_Audit-Policy_simple.j2
├── tests
│   ├── inventory
│   └── test.yml
└── vars
    └── main.yml
```
# TODO
* Full tests on nodes running Ubuntu 20.04 LTS or higher

