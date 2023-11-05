# HA proxy ingress controler 
This Ansible role installs HAproxy as Kubernetes ingress controller 

## Main steps to execute this Ansible playbook :
1. Possibly adjust variables in roles/dependencies/vars/main.yml (check comments in the file to understand effect of respective variables)
   
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

