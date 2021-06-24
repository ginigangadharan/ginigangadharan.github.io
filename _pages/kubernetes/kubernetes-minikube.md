---
layout: post
title: Minikube Installation
#author: gini
categories: [ kubernetes, minikube ]
#image: "assets/images/gini-redhat-cloudevent-2019-2.jpg"
tags: []
show-avatar: false
permalink: minikube
featured: false
hidden: false
titleshort: minikube
---

- [1. Introduction](#1-introduction)
- [2. Just to test ? Use this Lab](#2-just-to-test--use-this-lab)
- [3. Deploy minikube](#3-deploy-minikube)
  - [3.1. Setup Docker or Virtualization](#31-setup-docker-or-virtualization)
  - [3.2. Install and Configure kubectl](#32-install-and-configure-kubectl)
  - [3.3. Download and Install minikube](#33-download-and-install-minikube)
  - [3.4. Configure to Run docker without `sudo`](#34-configure-to-run-docker-without-sudo)
  - [3.5. Start minikube](#35-start-minikube)
    - [3.5.1. Start minikube cluster](#351-start-minikube-cluster)
    - [3.5.2. Verify cluster information](#352-verify-cluster-information)
  - [3.6. Kubernetes Dashboard](#36-kubernetes-dashboard)
    - [3.6.1. If you are running minikube inside a VM](#361-if-you-are-running-minikube-inside-a-vm)
  - [3.7. Get your hands dirty !](#37-get-your-hands-dirty-)
    - [3.7.1. If you are on Remote VM](#371-if-you-are-on-remote-vm)
  - [3.8. LoadBalancer deployments](#38-loadbalancer-deployments)
  - [3.9. Finished Testing ?](#39-finished-testing-)
- [4. Troubleshooting](#4-troubleshooting)
  - [4.1. `minikube start` exits with error on `GUEST_MISSING_CONNTRACK`](#41-minikube-start-exits-with-error-on-guest_missing_conntrack)
- [5. References](#5-references)

# 1. Introduction

**Note: Before you start minikube cluster**

minikube will need a hypervisor (VirtualBox or KVM) but if you are already inside a virtual machine (and nested virtualization is not possible), then it is possible to skip the creation of an additional VM layer by using the none driver. But please note, you need to install and setup docker environment in that vm.

To make none the default driver:

```shell
$ sudo minikube config set vm-driver none
❗  These changes will take effect upon a minikube delete and then a minikube start

## You can also mention the kubernetes version you need to install.
$ minikube start --kubernetes-version v1.16.0
## or 
$ minikube start --vm-driver=docker --wait=false \ 
  --kubernetes-version=v1.18.3
```

# 2. Just to test ? Use this Lab

There is a simple lab to see and test minikube; use it **[here](https://kubernetes.io/docs/tutorials/hello-minikube/#create-a-minikube-cluster)**


# 3. Deploy minikube

## 3.1. Setup Docker or Virtualization

Here we are using docker instead of Virtualization (VirtualBox etc.). Hence we need to install docker for the same.

Below steps are for Debian and you can refer [documentation](https://docs.docker.com/engine/install/) for other operating systems.(or [Install Docker on Debian](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-debian-9))

```shell
## remove any existing package
$ sudo apt-get remove docker docker-engine docker.io containerd runc

## Update the apt package index and install packages 
## to allow apt to use a repository over HTTPS:
$ sudo apt-get update
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release

## Add Docker’s official GPG key:
$ curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg  

$  echo \
  "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

## Install docker and tools
$ sudo apt-get update
$ sudo apt-get install docker-ce docker-ce-cli containerd.io
```

## 3.2. Install and Configure kubectl

You need `kubectl` to manage your cluster resources.

```
## Download kubectl
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

## Download the kubectl checksum file and Validate
$ curl -LO "https://dl.k8s.io/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"
$ echo "$(<kubectl.sha256) kubectl" | sha256sum --check

## Install 
$ sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```

*Note: if you have issue with `sudo` access or other thing try below.*

```shell
# Make the kubectl binary executable.
chmod +x ./kubectl
# Move the binary in to your PATH.
sudo mv ./kubectl /usr/local/bin/kubectl
```

**Test to ensure the version you installed is up-to-date:**

```shell
kubectl version --client
```

## 3.3. Download and Install minikube

```
# download and install package
$ curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
 
$ sudo install minikube-linux-amd64 /usr/local/bin/minikube

# check version
$ minikube version
minikube version: v1.21.0
commit: 76d74191d82c47883dc7e1319ef7cebd3e00ee11
```

## 3.4. Configure to Run docker without `sudo`

Add your user to the `docker` group: 

```shell
$ sudo usermod -aG docker $USER && newgrp docker
```

## 3.5. Start minikube 


### 3.5.1. Start minikube cluster

```
$ sudo minikube start --vm-driver=none --wait=false
😄  minikube v1.5.2 on Debian 9.11
🤹  Running on localhost (CPUs=2, Memory=7483MB, Disk=10013MB) ...
ℹ️   OS release is Debian GNU/Linux 9 (stretch)
⚠️  VM may be unable to resolve external DNS records
🐳  Preparing Kubernetes v1.16.2 on Docker '19.03.5' ...
💾  Downloading kubelet v1.16.2
💾  Downloading kubeadm v1.16.2
🚜  Pulling images ...
🚀  Launching Kubernetes ... 
🤹  Configuring local host environment ...

⚠️  The 'none' driver provides limited isolation and may reduce system security and reliability.
⚠️  For more information, see:
👉  https://minikube.sigs.k8s.io/docs/reference/drivers/none/

⚠️  kubectl and minikube configuration will be stored in /root
⚠️  To use kubectl or minikube commands as your own user, you may need to relocate them. For example, to overwrite your own settings, run:

    ▪ sudo mv /root/.kube /root/.minikube $HOME
    ▪ sudo chown -R $USER $HOME/.kube $HOME/.minikube

💡  This can also be done automatically by setting the env var CHANGE_MINIKUBE_NONE_USER=true
⌛  Waiting for: apiserver
🏄  Done! kubectl is now configured to use "minikube"
💡  For best results, install kubectl: https://kubernetes.io/docs/tasks/tools/install-kubectl/

```

Now we have a running kubernetes cluster and let us see the status.

### 3.5.2. Verify cluster information

```
$ kubectl cluster-info

$ kubectl get nodes
NAME       STATUS   ROLES                  AGE   VERSION
minikube   Ready    control-plane,master   15m   v1.20.7
```

## 3.6. Kubernetes Dashboard

You can enable the dashboard as below. (Take another console to run it)

```
$ sudo minikube dashboard
🤔  Verifying dashboard health ...
🚀  Launching proxy ...
🤔  Verifying proxy health ...
http://127.0.0.1:39067/api/v1/namespaces/kubernetes-dashboard/services/http:kubernetes-dashboard:/proxy/
```

Usually `minikube` will open the browser with the dashboard URL if you are with GUI. Access the url and check. 

### 3.6.1. If you are running minikube inside a VM

Then accessing Dashboard from host machine (or from your laptop) is bit tricky. By default minikube dashboard will exposed as `localhost:PORT`.

In above example, I have deployed the minikube inside a GCP (Google Cloud Platform) instance and see below how I am accessing the Dashboard.

**on minikube VM**

```
## 
$ minikube dashboard
🤔  Verifying dashboard health ...
🚀  Launching proxy ...
🤔  Verifying proxy health ...
🎉  Opening http://127.0.0.1:36959/api/v1/namespaces/kubernetes-dashboard/services/http:kubernetes-dashboard:/proxy/ in your default browser...
👉  http://127.0.0.1:36959/api/v1/namespaces/kubernetes-dashboard/services/http:kubernetes-dashboard:/proxy/
```

**On my Laptop/Workstation**

```shell
## Open an SSH tunnel 
## ssh -L LOCAL_PORT:localhost:REMOTE_PORT user@REMOTE_VM_IP
$ ssh -L 36959:localhost:36959 gini@123.123.234.234
.
.
gini@minikube-vm:~$ 
```

Open a browser and access the same url (output of `minikube dashboard` command)

`http://127.0.0.1:36959/api/v1/namespaces/kubernetes-dashboard/services/http:kubernetes-dashboard:/proxy/`

Bingo !

## 3.7. Get your hands dirty !

```shell
## Deploy an app
$ kubectl create deployment hello-minikube --image=k8s.gcr.io/echoserver:1.4
$ kubectl expose deployment hello-minikube --type=NodePort --port=8080

$ kubectl get services hello-minikube

## let minikube open the browser
$ minikube service hello-minikube

## Alternatively, use kubectl to forward the port:
kubectl port-forward service/hello-minikube 7080:8080
```

### 3.7.1. If you are on Remote VM

```shell
$ ssh -L 7080:localhost:7080 gini@123.123.234.234
```
And access the url fromlocal  browser - `localhost:7080`

## 3.8. LoadBalancer deployments

To access a LoadBalancer deployment, use the “minikube tunnel” command. Here is an example deployment:

```shell
kubectl create deployment balanced --image=k8s.gcr.io/echoserver:1.4  
kubectl expose deployment balanced --type=LoadBalancer --port=8080


## In another window, start the tunnel to create a routable IP for the ‘balanced’ deployment:

minikube tunnel
To find the routable IP, run this command and examine the EXTERNAL-IP column:

kubectl get services balanced
```

Your deployment is now available at <EXTERNAL-IP>:8080

## 3.9. Finished Testing ?

```shell
## Pause Kubernetes without impacting deployed applications
$ minikube pause

## Halt the cluster:
$ minikube stop
``` 

# 4. Troubleshooting 

## 4.1. `minikube start` exits with error on `GUEST_MISSING_CONNTRACK`

**Error**
❌  Exiting due to GUEST_MISSING_CONNTRACK: Sorry, Kubernetes 1.20.7 requires conntrack to be installed in root's path

**Solution**

```shell
sudo apt-get install -y conntrack
```

# 5. References

- https://github.com/kubernetes/minikube
- https://minikube.sigs.k8s.io/docs/start/