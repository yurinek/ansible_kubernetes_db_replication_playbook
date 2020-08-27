# ansible_kubernetes_db_replication_playbook

## Goals of the project

This project remotely deploys Postgres database replication over multiple Kubernetes pods/containers in a deployment or statefulset with the automation help of Ansible. <br>

## How to install

Prerequisit is Docker install <br>
clone https://github.com/yurinek/ansible_docker_db_replication_playbook, add your hosts to inventory_production/myapp_production and run 
```hcl
cd ansible_docker_db_replication_playbook
ansible-playbook -i inventory_production/myapp_production docker_install.yml
```

Install k8s on every node (this will not remove existing cluster) <br>
useful to run if there is something missing, it will install it without touching existing cluster
```hcl
ansible-playbook -i inventory_production/myapp_production kubernetes_install_k8s_install.yml
```
Install k8s on every node (this will remove existing cluster)
```hcl
ansible-playbook -i inventory_production/myapp_production kubernetes_install_k8s_install.yml -e 'k8s_reset_previous_cluster="yes"'
```

Configure cluster while connecting to master node
```hcl
ansible-playbook -i inventory_production/myapp_production kubernetes_install_k8s_cluster.yml -l myapp_production_master 
```

Install Postgresql Database replication in Kubernetes deployment or statefulset (default is deployment)
```hcl
ansible-playbook -i inventory_production/myapp_production kubernetes_db_replication_deploy.yml -l myapp_production_master -e 'k8s_object_to_update="deployment"'   
ansible-playbook -i inventory_production/myapp_production kubernetes_db_replication_deploy.yml -l myapp_production_master -e 'k8s_object_to_update="statefulset"' 
```

Uninstall Postgresql Database replication in Kubernetes deployment or statefulset (default is deployment)
```hcl
ansible-playbook -i inventory_production/myapp_production kubernetes_db_replication_destroy.yml -l myapp_production_master -e 'k8s_object_to_update="deployment"'   
ansible-playbook -i inventory_production/myapp_production kubernetes_db_replication_destroy.yml -l myapp_production_master -e 'k8s_object_to_update="statefulset"'   
```


