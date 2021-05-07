---
layout: post
title: How to set number of pods per node in OpenShift ?
author: gini
categories: [ OpenShift, kubernetes ]
tags: [ OpenShift, kubernetes ]
image: "assets/images/2020/how-to-create-increase-or-decrease-project-quota-in-openshift.jpg"
show-avatar: false
permalink: how-to-set-number-of-pods-per-node-openshift
featured: false
hidden: false
---

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