# Kubernetes-cluster-deployment 
This set of Ansible roles installs a Kubernetes cluster composed of 1 master and N workers.   
Note that the playbook detects if target OS is Ubuntu or Redhat/Centos/Rocky-Linux and runs respective tasks.   
So there are variables specific to each OS.   

Fully tested using :
* Ansible 2.10.8 on Ubuntu 22.04 LTS
* Rocky-linux 9.2 as Kubernetes cluster nodes (so it should work on CentOS 9 and RedHat 9 as well)
* CRI-O 1.24 as container runtime
* Kubernetes 1.27
* Calico as Container Network Interface (CNI), but ready soon to install Antrea, Cilium or Weave instead (see roles/cni/README.md)
* HAproxy as ingress controller

## Main steps to execute this Ansible playbook :
1. On each target system, ensure you have an account allowed to sudo ALL with password
2. Ensure you have a pair of SSH keys authorized on this account on each target systems (in ~/.ssh/authorized_keys)
3. On the Ansible host, unlock the respective SSH private key using the following commands :
```
    $ ssh-agent
    or
    $ eval $(ssh-agent)
    $ ssh-add <path-to-the-private-key>
        ... when prompted enter the passphrase
```  
4. Go to parent directory of this playbook
5. Edit host repository file to specify target hosts and username created in step 1 
   Please, list 1 master node, only; the playbook doesn't add master nodes, yet.
   List as many worker nodes as needed.
```
    $ vi hosts_inventory.yml 
``` 
6. Follow the instructions in each role/<role>/README.md file

7. To manage selinux, Ansible requires the installation of a module using the following command :
```
    $ ansible-galaxy collection install ansible.posix
```
   
8. Run the playbook
```
    $ ansible-playbook -i hosts_inventory.yml Kubernetes_masters_workers.yml -K
        ... when prompted, enter the target account passphrase (as used by sudo)
```
9. After the run has completed without error, log onto the master node using the account created at step 1 and use kubectl to manage the cluster. The kubeconfig file is copied locally, too, so that you can manage the cluster from your Ansible host using the following command :
```
    $ kubectl --kubeconfig <master node FQDN>_admin.conf <other arguments>
    Note : <master node hostname or IP>_admin.conf is retrieved locally by the playbook.
           The directory is specified in variable k8s_kubeconfig_dir (in role/k8s/var/main.yml)
```

## Directory structure :
```
.
├── hosts_inventory.yml
├── Kubernetes_masters_workers.yml
├── README.md
└── roles
    ├── auditing
    │   ├── defaults
    │   │   └── main.yml
    │   ├── files
    │   ├── handlers
    │   │   └── main.yml
    │   ├── meta
    │   │   └── main.yml
    │   ├── README.md
    │   ├── tasks
    │   │   └── main.yml
    │   ├── templates
    │   │   └── B13_K8S_Audit-Policy_simple.j2
    │   ├── tests
    │   │   ├── inventory
    │   │   └── test.yml
    │   └── vars
    │       └── main.yml
    ├── cni
    │   ├── defaults
    │   │   └── main.yml
    │   ├── files
    │   ├── handlers
    │   │   └── main.yml
    │   ├── meta
    │   │   └── main.yml
    │   ├── README.md
    │   ├── tasks
    │   │   ├── main.yml
    │   │   ├── rocky-linux_master.yml
    │   │   ├── rocky-linux_workers.yml
    │   │   └── ubuntu_master.yml
    │   ├── templates
    │   │   ├── calico_crd.j2
    │   │   └── calico_IPPools.j2
    │   ├── tests
    │   │   ├── inventory
    │   │   └── test.yml
    │   └── vars
    │       └── main.yml
    ├── cri-o
    │   ├── defaults
    │   │   └── main.yml
    │   ├── files
    │   ├── handlers
    │   │   └── main.yml
    │   ├── meta
    │   │   └── main.yml
    │   ├── README.md
    │   ├── tasks
    │   │   ├── main.yml
    │   │   ├── rocky-linux.yml
    │   │   └── ubuntu.yml
    │   ├── templates
    │   │   ├── 100-crio-bridge.conf.j2
    │   │   ├── crio.conf.j2
    │   │   └── registries.conf.j2
    │   ├── tests
    │   │   ├── inventory
    │   │   └── test.yml
    │   └── vars
    │       └── main.yml
    ├── ingress
    │   ├── defaults
    │   │   └── main.yml
    │   ├── files
    │   ├── handlers
    │   │   └── main.yml
    │   ├── meta
    │   │   └── main.yml
    │   ├── README.md
    │   ├── tasks
    │   │   ├── main.yml
    │   │   ├── rocky-linux_master.yml
    │   │   ├── rocky-linux_workers.yml
    │   │   └── ubuntu_master.yml
    │   ├── templates
    │   ├── tests
    │   │   ├── inventory
    │   │   └── test.yml
    │   └── vars
    │       └── main.yml
    └── k8s
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
* Full tests with Kubernetes nodes running Ubuntu 20.04 LTS or higher
* Full tests with Cilium CDN on both OS family
* Full tests with Weave CDN on both OS family
* Role to apply CIS Benchmarks 1.23 recommandations

