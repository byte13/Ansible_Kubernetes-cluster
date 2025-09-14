# Kubernetes-cluster-deployment 
This set of Ansible roles installs a Kubernetes cluster composed of 1 master and N workers.   
Note that the playbook detects if target OS is Ubuntu or Redhat/Centos/Rocky-Linux and runs respective tasks.   
So there are variables specific to each OS.   

Fully tested using :
* Ansible 2.16.3 on Ubuntu 24.04 LTS
* Rocky-linux 9.6 as Kubernetes cluster nodes (so it should work on CentOS 9 and RedHat 9 as well)
* CRI-O 1.32 as container runtime
* Kubernetes 1.32
* Calico as Container Network Interface (CNI) 
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
6. Follow the instructions in goup_vars/all and in each roles/\<role\>/README.md file

7. To manage selinux, Ansible requires the installation of a module using the following command :
```
    $ ansible-galaxy collection install ansible.posix
```
   
8. Run the playbook
```
    $ ansible-playbook -i hosts_inventory.yml Kubernetes_masters_workers.yml -K
        ... when prompted, enter the target account SSH passphrase (as used by sudo)
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
├── group_vars
│   └── all
├── hosts_inventory.yml
├── Kubernetes_masters_workers.yml
├── LICENSE
├── README.md
└── roles
    ├── auditing
    │   ├── defaults
    │   │   └── main.yml
    │   ├── handlers
    │   │   └── main.yml
    │   ├── meta
    │   │   └── main.yml
    │   ├── README.md
    │   ├── tasks
    │   │   └── main.yml
    │   ├── templates
    │   │   ├── K8S_Audit-Policy_example.j2
    │   │   ├── K8S_Audit-Policy_IBM-example.j2
    │   │   └── K8S_Audit-Policy_simple.j2
    │   ├── tests
    │   │   ├── inventory
    │   │   └── test.yml
    │   └── vars
    │       └── main.yml
    ├── certmanager
    │   ├── defaults
    │   │   └── main.yml
    │   ├── handlers
    │   │   └── main.yml
    │   ├── meta
    │   │   └── main.yml
    │   ├── README.md
    │   ├── tasks
    │   │   ├── main.yml
    │   │   ├── rocky-linux_master.yml
    │   │   └── rocky-linux_workers.yml
    │   ├── templates
    │   │   └── certmanager-values.j2
    │   ├── tests
    │   │   ├── inventory
    │   │   └── test.yml
    │   └── vars
    │       └── main.yml
    ├── cni
    │   ├── defaults
    │   │   └── main.yml
    │   ├── handlers
    │   │   └── main.yml
    │   ├── meta
    │   │   └── main.yml
    │   ├── README.md
    │   ├── tasks
    │   │   ├── main.yml
    │   │   ├── rocky-linux_masters_calico.yml
    │   │   ├── rocky-linux_masters_cilium.yml
    │   │   ├── rocky-linux_workers_calico.yml
    │   │   └── rocky-linux_workers_cilium.yml
    │   ├── templates
    │   │   ├── calico.conf.j2
    │   │   ├── calico_crd.j2
    │   │   └── calico_ippools.j2
    │   ├── tests
    │   │   ├── inventory
    │   │   └── test.yml
    │   └── vars
    │       └── main.yml
    ├── cri
    │   ├── defaults
    │   │   └── main.yml
    │   ├── handlers
    │   │   └── main.yml
    │   ├── meta
    │   │   └── main.yml
    │   ├── README.md
    │   ├── tasks
    │   │   ├── main.yml
    │   │   ├── rocky-linux_containerd.yml
    │   │   └── rocky-linux_cri-o.yml
    │   ├── templates
    │   │   ├── 100-cri-bridge.conf.j2
    │   │   ├── crio.conf.j2
    │   │   └── registries.conf.j2
    │   ├── tests
    │   │   ├── inventory
    │   │   └── test.yml
    │   └── vars
    │       └── main.yml
    ├── dependencies
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
    │   │   └── rocky-linux_workers.yml
    │   ├── templates
    │   ├── tests
    │   │   ├── inventory
    │   │   └── test.yml
    │   └── vars
    │       └── main.yml
    ├── fluentbit
    │   ├── defaults
    │   │   └── main.yml
    │   ├── handlers
    │   │   └── main.yml
    │   ├── meta
    │   │   └── main.yml
    │   ├── README.md
    │   ├── tasks
    │   │   ├── main.yml
    │   │   ├── rocky-linux_master.yml
    │   │   └── rocky-linux_workers.yml
    │   ├── templates
    │   │   ├── fluent-bit-configmap.yaml
    │   │   └── fluentbit-values.j2
    │   ├── tests
    │   │   ├── inventory
    │   │   └── test.yml
    │   └── vars
    │       └── main.yml
    ├── gatewayapi-crds
    │   ├── defaults
    │   │   └── main.yml
    │   ├── handlers
    │   │   └── main.yml
    │   ├── meta
    │   │   └── main.yml
    │   ├── README.md
    │   ├── tasks
    │   │   ├── main.yml
    │   │   ├── rocky-linux_master.yml
    │   │   └── rocky-linux_workers.yml
    │   ├── templates
    │   │   └── kong-ingress-values.j2
    │   ├── tests
    │   │   ├── inventory
    │   │   └── test.yml
    │   └── vars
    │       └── main.yml
    ├── gitops
    │   ├── defaults
    │   │   └── main.yml
    │   ├── handlers
    │   │   └── main.yml
    │   ├── meta
    │   │   └── main.yml
    │   ├── README.md
    │   ├── tasks
    │   │   ├── main.yml
    │   │   ├── rocky-linux_master.yml
    │   │   └── rocky-linux_workers.yml
    │   ├── templates
    │   │   └── namespace_gitops.yaml.j2
    │   ├── tests
    │   │   ├── inventory
    │   │   └── test.yml
    │   └── vars
    │       └── main.yml
    ├── ingress-haproxy
    │   ├── defaults
    │   │   └── main.yml
    │   ├── handlers
    │   │   └── main.yml
    │   ├── meta
    │   │   └── main.yml
    │   ├── README.md
    │   ├── tasks
    │   │   ├── main.yml
    │   │   ├── rocky-linux_master.yml
    │   │   └── rocky-linux_workers.yml
    │   ├── templates
    │   │   └── haproxy-ingress-values.j2
    │   ├── tests
    │   │   ├── inventory
    │   │   └── test.yml
    │   └── vars
    │       └── main.yml
    ├── ingress-kong
    │   ├── defaults
    │   │   └── main.yml
    │   ├── handlers
    │   │   └── main.yml
    │   ├── meta
    │   │   └── main.yml
    │   ├── README.md
    │   ├── tasks
    │   │   ├── main.yml
    │   │   ├── rocky-linux_master.yml
    │   │   └── rocky-linux_workers.yml
    │   ├── templates
    │   │   ├── kong-ingress-values.j2
    │   │   ├── kong-ingress-values_long.j2
    │   │   └── kong-ingress-values_short.j2
    │   ├── tests
    │   │   ├── inventory
    │   │   └── test.yml
    │   └── vars
    │       └── main.yml
    ├── ingress-nginx-fabric
    │   ├── defaults
    │   │   └── main.yml
    │   ├── handlers
    │   │   └── main.yml
    │   ├── meta
    │   │   └── main.yml
    │   ├── README.md
    │   ├── tasks
    │   │   ├── main.yml
    │   │   ├── rocky-linux_master.yml
    │   │   └── rocky-linux_workers.yml
    │   ├── templates
    │   ├── tests
    │   │   ├── inventory
    │   │   └── test.yml
    │   └── vars
    │       └── main.yml
    ├── k8s
    │   ├── defaults
    │   │   └── main.yml
    │   ├── handlers
    │   │   └── main.yml
    │   ├── meta
    │   │   └── main.yml
    │   ├── README.md
    │   ├── tasks
    │   │   ├── main.yml
    │   │   ├── rocky-linux_master.yml
    │   │   └── rocky-linux_workers.yml
    │   ├── templates
    │   │   ├── cluster-kubelet-config.j2
    │   │   └── kubelet.j2
    │   ├── tests
    │   │   ├── inventory
    │   │   └── test.yml
    │   └── vars
    │       └── main.yml
    ├── keycloak
    │   ├── defaults
    │   │   └── main.yml
    │   ├── handlers
    │   │   └── main.yml
    │   ├── meta
    │   │   └── main.yml
    │   ├── README.md
    │   ├── tasks
    │   │   └── main.yml
    │   ├── templates
    │   ├── tests
    │   │   ├── inventory
    │   │   └── test.yml
    │   └── vars
    │       └── main.yml
    ├── oidc
    │   ├── defaults
    │   │   └── main.yml
    │   ├── handlers
    │   │   └── main.yml
    │   ├── meta
    │   │   └── main.yml
    │   ├── README.md
    │   ├── tasks
    │   │   └── main.yml
    │   ├── templates
    │   ├── tests
    │   │   ├── inventory
    │   │   └── test.yml
    │   └── vars
    │       └── main.yml
    └── rancher
        ├── defaults
        │   └── main.yml
        ├── handlers
        │   └── main.yml
        ├── meta
        │   └── main.yml
        ├── README.md
        ├── tasks
        │   ├── main.yml
        │   ├── rocky-linux_master.yml
        │   └── rocky-linux_workers.yml
        ├── templates
        │   └── rancher-values.j2
        ├── tests
        │   ├── inventory
        │   └── test.yml
        └── vars
            └── main.yml

```
# TODO
* Full tests with Cilium CNI
* Role to apply CIS Benchmarks

