# Ansible playbooks to deploy jrobot in K8s (VMM enviroment)

Build a kubernetes cluster using Ansible via kubeadm


Requirements:
	
	- Ansible `2.4.0+`
	
	- Master and nodes must have passwordless SSH access


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




