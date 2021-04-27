---
layout: post
title: OpenShift & Kubernetes - Questions and References
author: gini
categories: [ OpenShift, kubernetes ]
tags: [ OpenShift, kubernetes ]
image: "assets/images/2020/how-to-create-increase-or-decrease-project-quota-in-openshift.jpg"
show-avatar: false
permalink: openshift-kubernetes-questions
featured: false
hidden: false
titleshort: OpenShift Questions
---

*Note: These are questions I have received via chat groups and communities. This is a living document and I will update the page whenever there is a new question or better answer or references.*

- [How to set number of pods per node in OpenShift ?](#how-to-set-number-of-pods-per-node-in-openshift-)
- [TODO/ What is ODF ?](#todo-what-is-odf-)
- [TODO/ What is Kubernetes Admission Controller ?](#todo-what-is-kubernetes-admission-controller-)
- [How Container Security Works ?](#how-container-security-works-)
- [What is Air-Gapped (disconnected) OpenShift Clusters](#what-is-air-gapped-disconnected-openshift-clusters)
- [TODO/ What is CoreOS and Why CoreOS for OpenShift ?](#todo-what-is-coreos-and-why-coreos-for-openshift-)
- [TODO/ What is `etcd` ?](#todo-what-is-etcd-)
- [TODO/ What is Kubernetes Operator (or OpenShift Operator) ?](#todo-what-is-kubernetes-operator-or-openshift-operator-)
- [TODO/ OpenShift Logging](#todo-openshift-logging)
- [How to Manage Roles and Permissons in OpenShift ?](#how-to-manage-roles-and-permissons-in-openshift-)
- [What is SRE and DevOps ?](#what-is-sre-and-devops-)
- [How to Enabled OpenShift Node AutoScaling ?](#how-to-enabled-openshift-node-autoscaling-)
- [Reference](#reference)

## How to set number of pods per node in OpenShift ?

```bash
## get the machineconfigpool
$ oc get machineconfigpool

## see details
$ oc describe machineconfigpool worker

## add label to the machineconfigpool if not exist
$ oc label machineconfigpool worker custom-kubelet=small-pods
#eg:
apiVersion: machineconfiguration.openshift.io/v1
kind: MachineConfigPool
metadata:
  creationTimestamp: 2019-02-08T14:52:39Z
  generation: 1
  labels:
    custom-kubelet: small-pods 

## Create a custom resource (CR) for your configuration change.
## Setting podsPerCore to 0 disables this limit.
apiVersion: machineconfiguration.openshift.io/v1
kind: KubeletConfig
metadata:
  name: set-max-pods 
spec:
  machineConfigPoolSelector:
    matchLabels:
      custom-kubelet: small-pods 
  kubeletConfig:
    podsPerCore: 10 
    maxPods: 250 

## check the machines and wait for update
$ oc get machineconfigpool 
## eg:
NAME     CONFIG                        UPDATED   UPDATING   DEGRADED
master   master-9cc2c72f205e103bb534   False     False      False
worker   worker-8cecd1236b33ee3f8a5e   False     True       False
```

- [OpenShift Scale: Running 500 Pods Per Node](https://www.openshift.com/blog/500_pods_per_node)
- [Managing the maximum number of pods per node](https://docs.openshift.com/container-platform/4.6/nodes/nodes/nodes-nodes-managing-max-pods.html)

## TODO/ What is ODF ?

## TODO/ What is Kubernetes Admission Controller ?
- `MutatingAdmissionWebhook` and `ValidatingAdmissionWebhook`

- [Using Admission ControllersUsing Admission Controllers](https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/)

## How Container Security Works ?
-  Secure the container host
-  Secure the networking environment -  intrusion prevention system (IPS) and web filtering for traffic moving from north to south, and to and from the internet, in order to stop attacks and filter malicious content.
-  Secure your management stack - container registry, Kubernetes installation, network policies.
-  Build on a secure foundation - patching, updates
-  Secure your build pipeline
-  Secure your application

- [Container Security in Six Steps](https://www.trendmicro.com/vinfo/sg/security/news/security-technology/container-security-in-six-steps)


## What is Air-Gapped (disconnected) OpenShift Clusters

Air-gapped environments are those that are physically isolated from other networks, but most importantly isolated from the Internet. No proxies, no jump hosts - nothing. 

- [Is your Operator Air-Gap Friendly?](https://www.openshift.com/blog/is-your-operator-air-gap-friendly)
- OpenShift 4 in an Air Gap (disconnected) environment- [Part1]](https://two-oes.medium.com/openshift-4-in-an-air-gap-disconnected-environment-part-1-prerequisites-63f065e8a729), [Part2](https://two-oes.medium.com/openshift-4-in-an-air-gap-disconnected-environment-part-2-installation-1dd8bf085fdd)

## TODO/ What is CoreOS and Why CoreOS for OpenShift ?

CoreOS was founded in 2013 with the mission to improve the security and reliability of the internet. *On May 26, 2020, CoreOS Container Linux reached its end of life and will no longer receive updates.*

- [What was CoreOS](https://www.openshift.com/learn/topics/coreos) - 


## TODO/ What is `etcd` ?

`etcd` is a distributed key-value store.

## TODO/ What is Kubernetes Operator (or OpenShift Operator) ?

- [Troubleshooting OpenShift Operator issues](https://docs.openshift.com/container-platform/4.5/support/troubleshooting/troubleshooting-operator-issues.html)

## TODO/ OpenShift Logging

- [Installing OpenShift Logging](https://docs.openshift.com/container-platform/4.7/logging/cluster-logging-deploying.html)

## How to Manage Roles and Permissons in OpenShift ?

- [Using RBAC to define and apply permissions](https://docs.openshift.com/container-platform/4.7/authentication/using-rbac.html)

## What is SRE and DevOps ?

- [What is SRE (site reliability engineering)? DevOps vs. SRE](https://www.redhat.com/en/topics/devops/what-is-sre)

## How to Enabled OpenShift Node AutoScaling ?

- [Applying autoscaling to an OpenShift Container Platform cluster](https://docs.openshift.com/container-platform/4.7/machine_management/applying-autoscaling.html)

## Reference
