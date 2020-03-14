# NUC Lab Setup Script

This is a repository for devops automation to bootstrap a NUC via Ansible to create an Ansible cluster and install a Prometheus and Graphana instance.

## Background

Ansible is used to deploy the cluster as well as the appropriate Kubernetes resources.
In the cluster there are 3 total Intel NUC machines. 
Both had Ubuntu 18.04 setup with the username *brett* (SSH keys cloned from Launchpad).
One NUC is a master node and two NUCs are setup as "workers".
Gitlab is used as the git repository. Possibly it will be mirrored to Github.
Development and Ansible master is done via a Windows desktop using VSCode WSL integration.
Currently nodeports are used to access the cluster.

## Setup
For the Ansible config to be used with WSL we need to run `export ANSIBLE_CONFIG=./ansible.cfg` more info here https://docs.ansible.com/ansible/latest/reference_appendices/config.html#avoiding-security-risks-with-ansible-cfg-in-the-current-directory and here https://www.asyncdrink.com/blog/ansible-on-windows

To setup the cluster, clone the git repository and run the **setup_playbook.yaml**

The setup_playbook 
- Setup Docker
  - Disables SWAP and other requirements.
- Downloads kubeadm and runs it depending on the role in the Ansible inventories.
  - Setup Calico

To deploy the resources, run the **deploy_playbook.yaml**

- Run locally downloaded Prometheus Helm charts and deploy them
- Run locally downloaded Grafana Helm charts and deploy them
- Run locally downloaded Ingress Helm charts and deploy them
- Run manual hacks for each


## Todo
- [ ] Finish provising worker images with Ansible. (And to Ansible best practices.)
- [ ] Swap the local disks with Gluster
- [ ] Setup with a load balancer resource on baremetal. (METALLB looks promising)
- [ ] Setup KubeOps view
- [ ] Setup DNS for use on network (?)
- [ ] Setup IPAM and network diagram (Netbox would be a fun option.)
- [ ] Explore health monitoring options such as KuberHealthy
- [ ] Explore gitlab deployment options
- [ ] Setup Postgres for testing storing some metrics in a database.
- [ ] Setup Thanos and multipe Prometheus pods.