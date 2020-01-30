# Ansible playbooks to deploy jrobot in K8s (VMM enviroment)

Build a kubernetes cluster using Ansible via kubeadm in vmm environment


Requirements:
	
	- Ansible `2.4.0+`
	
	- Master and nodes must have passwordless SSH access

# Usage

Edit group_vars/vmm_pods.yml file with yours credentials:

```
ansible_connection: ssh 
ansible_ssh_user: "xxxxxx"
ansible_ssh_pass: "xxxxxx" 
ansible_ssh_common_args: '-o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no'
```

Run the following playbook to bring up vmm topolgy (1 master 2 workers):

```
ansible-playbook -i vmm_pods ./playbooks/deploy_vmm_topology.yml
```

Once vmm topology is up and running:

Edit `group_vars/all.yml` to your specified configuration.

For example, `flannel` as SDN

```
# Network implementation('flannel', 'calico')
network: flannel
```

Run the following playbook to bring up kubernetes cluster setup:
```
ansible-playbook ./playbooks/site.yml
```

In order to deploy jrobot in the cluster, edit ./groups_vars/kube_cluster.yml and put git credentials, then run the following playbook:

```
ansible-playbook ./playbooks/git-robot.yml
```
, this playbook will clone the project in the master node, then edit /root/robot/jrobot/values.yaml

and put the correct git credentials in the gitRepository section, then run:

```
ansible-playbook ./playbooks/deploy-robot.yml
```

# Resetting the environment

Reset all kubeadm installed using `reset-site.yaml` playbook:

```sh
ansible-playbook ./playbooks/reset-site.yaml
```





