---
layout: post
title: Install Ansible AWX using AWX Operator (on Kubernetes)
categories: [ ansible ]
tags: [ansible]
show-avatar: false
permalink: ansible-awx-operator
featured: false
hidden: false
showindex: true
---

- [Install Ansible AWX using AWX Operator (on Kubernetes)](#install-ansible-awx-using-awx-operator-on-kubernetes)
  - [Setup a Kubernete s Cluster](#setup-a-kubernete-s-cluster)
  - [Deploy AWX Operator](#deploy-awx-operator)
  - [Create AWX Deployment](#create-awx-deployment)
  - [Get Admin Password](#get-admin-password)
  - [Get the Path to Access Ansible AWX](#get-the-path-to-access-ansible-awx)
    - [Access Ansible AWX on Remote Minikube or Kubernetes Cluster](#access-ansible-awx-on-remote-minikube-or-kubernetes-cluster)
- [References](#references)


# Install Ansible AWX using AWX Operator (on Kubernetes)

## Setup a Kubernete s Cluster
We will install it on a `minikube`. (Refere [minikube installation](minikube))

```shell
$ minikube start --addons=ingress --cni=flannel --install-addons=true \
    --kubernetes-version=stable \
    --vm-driver=docker --wait=false \
    --cpus=4 --memory=6g
```

**Enable Addons as needed**

```shell
## Check addons
$ minikube addons list
$ minikube addons enable metrics-server
$ minikube dashboard
```

## Deploy AWX Operator

Replace `TAG` with the version from [Release Page](https://github.com/ansible/awx-operator/releases)
```shell
kubectl apply -f https://raw.githubusercontent.com/ansible/awx-operator/<TAG>/deploy/awx-operator.yaml
```

## Create AWX Deployment 

Create a file `awx-demo.yaml`

```yaml
---
apiVersion: awx.ansible.com/v1beta1
kind: AWX
metadata:
  name: awx-demo
spec:
  service_type: nodeport
  ingress_type: none
  hostname: awx-demo.example.com
```  

```bash
$ kubectl apply -f awx-demo.yml
awx.awx.ansible.com/awx-demo created
```

Refer [AWX Operator](https://github.com/ansible/awx-operator) documentation for advanced installation options.

## Get Admin Password

By default, the admin user is `admin` and the password is available in the `<resourcename>-admin-password` secret.

```shell
$ kubectl get secret awx-demo-admin-password -o jsonpath="{.data.password}" | base64 --decode
lxQ8uWlE9Wevkgmy5Kx2AqFdY80v34gx
```

## Get the Path to Access Ansible AWX

You will find the `NodePort` and IP for service `awx-demo-service`

```shell
$ kubectl get svc
NAME                   TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)             AGE
awx-demo-postgres      ClusterIP   None             <none>        5432/TCP            23m
awx-demo-service       NodePort    10.110.146.161   <none>        80:31726/TCP        23m
awx-operator-metrics   ClusterIP   10.104.113.88    <none>        8383/TCP,8686/TCP   23m
kubernetes             ClusterIP   10.96.0.1        <none>        443/TCP             26m
```
Access Ansible AWX Portal at `IP_ADDRESS:NODE_PORT`.

Or let `minikube` open the browser

```shell
$ minikube service hello-minikube
```

### Access Ansible AWX on Remote Minikube or Kubernetes Cluster

If your Kubernetes/minikube Cluster is on Remote Machine/VM (eg: Cloud Instance with Public IP) then you can access it using above method (if NodePort is same is remote IP) or you can use LoadBalancer methods.

In our case, we have deplyed this in a Google Cloud instance without GUI and we need to enabled port-forwading as below.

```
## Forward localhost (minkube VM Localhost) port 7080 -> 80
$ kubectl port-forward service/awx-demo-service 7080:80
```
So the Ansible AWX service is available at minikube VM localhost:7080 now; but we dont have GUI there to access !!

So, we do a port-forwarding from our laptop/workstation via SSH Tunnel.

```shell
## On your Workstation/Laptop
## ssh -L LOCAL_PORT:localhost:REMOTE_PORT User@REMOTE_IP
$ ssh -L 7080:localhost:7080 gini@123.123.234.234
```

Now, open a browser on your laptop/workstation and goto `localhost:7080`; that's it.

Enjoy `Ansible AWX` running on top of `Kubernetes`/`minikube`.

# References