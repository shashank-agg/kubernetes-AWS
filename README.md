
# Creating a kubernetes cluster on AWS using kubeadm and ansible.

### Prerequisites:

1. Create 3 EC2 instances on AWS (any type), and download their SSH key(s).

2. Install ansible on your local machine. Ansible is used to run shell commands on a remote machine, and internally uses SSH for communication.



### Steps
1. Create a ```hosts``` file on your local machine. This contains the IP addresses of the 3 AWS machines, partitioned into two groups - master and workers. This file (called the inventory file) is used by ansible to determine all machines which it controls/operates on.

2. Create a file ```kube-dependencies.yml```. This file instructs ansible to install all Kubernetes dependencies such as Docker, kubeadm, kubectl etc. on all the 3 machines. Then, run it using 
```ansible-playbook -i hosts kube-dependencies.yml```

3. Create a file ```master.yml```. This file instructs ansible to perform Kubeadm initialisation only on the **master** node. Then, run it using 
```ansible-playbook -i hosts master.yml```

4. Create a file ```workers.yml```. This file instructs ansible to first get the "cluster join" command from the master, and then run it on the 2 worker nodes. Run it using 
```ansible-playbook -i hosts workers.yml```

That's it. You can now SSH into the master node, and run ```kubectl get nodes``` to verify if the master can detect the 2 workers nodes.
