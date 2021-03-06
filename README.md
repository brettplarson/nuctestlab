<img src="generic_PC_intel-NUC.svg" width="124" height="124">

# NUC Lab Setup Script
This is a repository for devops automation to bootstrap a NUC via Ansible to create an Kubernetes cluster and install a Prometheus and Graphana instance.

## Background
Ansible is used to deploy the cluster as well as the appropriate Kubernetes resources.
In the cluster there are 3 total Intel NUC machines. 
Both have had Ubuntu 18.04 server setup with the username *brett* with no extra setup (besides the SSH keys cloned from Launchpad :)).
One NUC is a master node and two NUCs are setup as "workers".
Gitlab is used as the git repository. Possibly it will be mirrored to Github.
Development and the Ansible controller is done via a Windows desktop using VSCode WSL integration.
Currently nodeports are used to access the cluster.

## Setup
### Setup Ansible
For the Ansible config to be used with WSL we need to run `export ANSIBLE_CONFIG=./ansible.cfg` 
More info [here](https://docs.ansible.com/ansible/latest/reference_appendices/config.html#avoiding-security-risks-with-ansible-cfg-in-the-current-directory) and [here](https://www.asyncdrink.com/blog/ansible-on-windows).

Secondly we need to first run the playbook to setup Ansible to allow sudo without passwords for the *brett* user for all of the hosts.
`ansible-playbook ansible_setup.yaml -K`

Finally on the Ansible controller we want to install the [Ansible Kubernetes module.](https://docs.ansible.com/ansible/latest/scenario_guides/guide_kubernetes.html)
`ansible-galaxy install ansible.kubernetes-modules`

### Run playbooks
To setup the cluster run the **setup_playbook.yaml**

The setup_playbook does the following:
- Setup Docker
  - Disables Swap
  - Add repos for Kubernetes and Docker
  - Install K8s and Docker
  - Use systemd as the Cgroup driver. [More info](https://kubernetes.io/docs/setup/production-environment/container-runtimes/#cgroup-drivers)
- Downloads kubeadm and runs it depending on the role in the Ansible inventories.
  - Setup Calico

To deploy the resources run the **deploy_playbook.yaml**

- Run locally downloaded Prometheus Helm charts and deploy them
- Run locally downloaded Grafana Helm charts and deploy them
- Run locally downloaded Ingress Helm charts and deploy them
- Run manual hacks for each
- Enjoy the beautiful pods running in kopsview!

<img src="kubeops.png" width="124" height="124">

## Todo
- [x] Finish provising worker images with Ansible. (And to Ansible best practices.)
- [x] Setup glustefs on master (standalone)
- [x] Swap the local disks with Gluster
- [x] Setup with a load balancer resource on baremetal. (METALLB looks promising)
- [x] Setup KubeOps view
- [x] Setup DNS for use on network (?)
- [ ] Install and setup Istio
- [ ] Setup IPAM and network diagram (Netbox would be a fun option.)
- [ ] Explore health monitoring options such as KuberHealthy
- [ ] Explore gitlab / github deployment options
- [ ] Setup Postgres for testing storing some metrics in a database.
- [ ] Setup Thanos and multipe Prometheus pods.

## Thanos
- [ ] Finish GKE Terraform setup for a small test cluster
- [ ] Setup Thanos in GKE
- [ ] Add Thanos sidecar to NUC servers
- [ ] Configure gRPC 