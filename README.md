# Kubernetes Ansible with KubeAdm

Quick and dirty ansible for deploying kubernetes with kubeadm.

Based on this guide https://www.arubacloud.com/tutorial/how-to-create-kubernetes-cluster-with-kubeadm-and-ansible-ubuntu-20-04.aspx

## Status

- [X] Basic ansible to deploy a cluster with a single master (prepared for HA)
- [ ] HA Control Plane
- [ ] Removal of hardcoded variables such as Service/Pod CIDR Ranges

## Components

For CNI we use [kube-router](https://www.kube-router.io).

Rather than using Docker for the container runtime, we use [containerd](https://containerd.io/).

We are using kube-router to expose ClusterIP services to the network, running a highly available control plane, this is W.I.P.

# Deployment phases

## Pre-reqs

Create an ansible inventory in hosts. Each host should be running Ubuntu 20.04.

Generate a SSH key

`ssh-keygen -t rsa -m PEM -f npf-ansible -C "npf-ansible"`

Add SSH key to agent
`ssh-add npf-ansible`

Copy SSH key to each host
`ssh-copy-id -i npf-ansible npf@172.20.22.11`

## Phase 1: Initial setup

Execute ansible playbook initial
`ansible-playbook -i hosts initial.yml --ask-become-pass`

## Phase 2: Install kubernetes dependencies

Execute ansible playbook deps
`ansible-playbook -i hosts deps.yml --ask-become-pass`

## Phase 3: Create kubernetes cluster

Execute ansible playbook deps
`ansible-playbook -i hosts kubeadm-init.yml --ask-become-pass`

## Phase 4: Join worker nodes to cluster

Execute ansible playbook deps
`ansible-playbook -i hosts kubeadm-join.yml --ask-become-pass`
