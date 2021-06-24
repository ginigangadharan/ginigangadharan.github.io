---
layout: post
title: Kubernetes Installation
#author: gini
categories: [ kubernetes ]
#image: "assets/images/gini-redhat-cloudevent-2019-2.jpg"
tags: []
show-avatar: false
permalink: /kubernetes-installation
featured: false
hidden: false
titleshort: Kubernetes Installation
---

- [1. kubernetes-installation-using-kubeadm](#1-kubernetes-installation-using-kubeadm)
- [2. Installing Kubernetes the Hardway](#2-installing-kubernetes-the-hardway)
  - [2.1. Install virtualbox and vagarnt](#21-install-virtualbox-and-vagarnt)
  - [2.2. then....](#22-then)
- [3. Kubernetes Installation using Ansible](#3-kubernetes-installation-using-ansible)


# 1. kubernetes-installation-using-kubeadm

- https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/#before-you-begin
- https://medium.com/containerum/4-ways-to-bootstrap-a-kubernetes-cluster-de0d5150a1e4

Upgrade the version of kubeadm on the master and node01 to 1.16.0
For easier installation, use apt-get install instead of upgrade

```
sudo apt-get update && sudo apt-get install -qy kubeadm=1.16.0-00
```

Initialize Control Plane Node (Master)

```
$ kubeadm init

Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 172.17.0.17:6443 --token baawxx.9ez921jerna2rb6f \
    --discovery-token-ca-cert-hash sha256:98cb7e30b0361da3e3eb689cd74af9288dfcc99d36c9b5541b1765f9a9f0bd20
```

If you do not have the token, you can get it by running the following command on the control-plane node:

```
kubeadm token list
```

By default, tokens expire after 24 hours. If you are joining a node to the cluster after the current token has expired, you can create a new token by running the following command on the control-plane node:

on node01
```
kubeadm join 172.17.0.17:6443 --token baawxx.9ez921jerna2rb6f \
    --discovery-token-ca-cert-hash sha256:98cb7e30b0361da3e3eb689cd74af9288dfcc99d36c9b5541b1765f9a9f0bd20
```

Install a Network Plugin. As a default, we will go with weave

```
kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"
```

# 2. Installing Kubernetes the Hardway

- [GCP](https://github.com/ginigangadharan/kubernetes-the-hard-way-gcp)
- [VirtualBox](https://github.com/ginigangadharan/kubernetes-the-hard-way-virtualbox)

## 2.1. Install virtualbox and vagarnt

## 2.2. then....

# 3. Kubernetes Installation using Ansible

- https://www.youtube.com/watch?v=kMKkpgB6IwM
- https://kubernetes.io/blog/2019/03/15/kubernetes-setup-using-ansible-and-vagrant/
- https://medium.com/faun/how-to-create-your-own-kubernetes-cluster-using-ansible-7c6b5c031a5d
- https://www.azavea.com/blog/2014/10/30/running-vagrant-with-ansible-provisioning-on-windows/
