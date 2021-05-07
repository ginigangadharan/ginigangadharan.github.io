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
- [What is a Distributed System ?](#what-is-a-distributed-system-)
- [TODO/ What is ODF ?](#todo-what-is-odf-)
- [TODO/ What is Kubernetes Admission Controller ?](#todo-what-is-kubernetes-admission-controller-)
- [How Container Security Works ?](#how-container-security-works-)
- [TODO/ What is Red Hat Nooba ?](#todo-what-is-red-hat-nooba-)
- [What is Air-Gapped (disconnected) OpenShift Clusters](#what-is-air-gapped-disconnected-openshift-clusters)
- [What is Clair ?](#what-is-clair-)
- [What is Quay ?](#what-is-quay-)
- [TODO/ What is CoreOS and Why using CoreOS for OpenShift ?](#todo-what-is-coreos-and-why-using-coreos-for-openshift-)
- [What is CRI-O ?](#what-is-cri-o-)
- [TODO/ What is `etcd` ?](#todo-what-is-etcd-)
- [TODO/ What is Kubernetes Operator (or OpenShift Operator) ?](#todo-what-is-kubernetes-operator-or-openshift-operator-)
- [TODO/ OpenShift Logging](#todo-openshift-logging)
- [How to Manage Roles and Permissons in OpenShift ?](#how-to-manage-roles-and-permissons-in-openshift-)
- [What is SRE and DevOps ?](#what-is-sre-and-devops-)
- [How to Enabled OpenShift Node AutoScaling ?](#how-to-enabled-openshift-node-autoscaling-)
- [Reference](#reference)

## How to set number of pods per node in OpenShift ?

- [How to set number of pods per node in OpenShift ? - Quick Guide](how-to-set-number-of-pods-per-node-openshift)
- [OpenShift Scale: Running 500 Pods Per Node](https://www.openshift.com/blog/500_pods_per_node)
- [Managing the maximum number of pods per node](https://docs.openshift.com/container-platform/4.6/nodes/nodes/nodes-nodes-managing-max-pods.html)

## What is a Distributed System ?

A distributed system in its most simplest definition is a group of computers working together as to appear as a single computer to the end-user. These machines have a shared state, operate concurrently and can fail independently without affecting the whole system’s uptime. 

Read more : [A Thorough Introduction to Distributed Systems](https://www.freecodecamp.org/news/a-thorough-introduction-to-distributed-systems-3b91562c9b3c/)

- Fault Tolerance / High Availability
- Low Latency 
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

## TODO/ What is Red Hat Nooba ?

## What is Air-Gapped (disconnected) OpenShift Clusters

Air-gapped environments are those that are physically isolated from other networks, but most importantly isolated from the Internet. No proxies, no jump hosts - nothing. 

- [Is your Operator Air-Gap Friendly?](https://www.openshift.com/blog/is-your-operator-air-gap-friendly)
- OpenShift 4 in an Air Gap (disconnected) environment- [Part1]](https://two-oes.medium.com/openshift-4-in-an-air-gap-disconnected-environment-part-1-prerequisites-63f065e8a729), [Part2](https://two-oes.medium.com/openshift-4-in-an-air-gap-disconnected-environment-part-2-installation-1dd8bf085fdd)

## What is Clair ?

Clair is an open source project for the static analysis of vulnerabilities in application containers (currently including OCI and docker).

- [Clair GitHub](https://github.com/quay/clair)

## What is Quay ?

Quay is a container image registry that enables you to build, organize, distribute, and deploy containers. This is the Community Distribution of Quay that powers Red Hat Quay and Quay.io

- [ProjectQuay][https://www.projectquay.io/]
  
## TODO/ What is CoreOS and Why using CoreOS for OpenShift ?

CoreOS was founded in 2013 with the mission to improve the security and reliability of the internet. *On May 26, 2020, CoreOS Container Linux reached its end of life and will no longer receive updates.*
-  It’s the successor to both Fedora Atomic Host and CoreOS Container Linux
-  Based on RHEL
-  Controlled immutability
-  CRI-O container runtime
-  Set of container tools: `podman`, `skopeo`, `buildah`, `crictl`

**References**
- [What was CoreOS](https://www.openshift.com/learn/topics/coreos)
- [Getting Started with CoreOS](https://www.digitalocean.com/community/tutorial_series/getting-started-with-coreos-2)
- [Producing an Ignition Config](https://docs.fedoraproject.org/en-US/fedora-coreos/producing-ign/)

## What is CRI-O ?

LIGHTWEIGHT CONTAINER RUNTIME FOR KUBERNETES

CRI-O is an implementation of the Kubernetes CRI (Container Runtime Interface) to enable using OCI (Open Container Initiative) compatible runtimes. It is a lightweight alternative to using Docker as the runtime for kubernetes. It allows Kubernetes to use any OCI-compliant runtime as the container runtime for running pods. 

- [Website](https://cri-o.io/)
- 
## TODO/ What is `etcd` ?

`etcd` is a distributed key-value store.

## TODO/ What is Kubernetes Operator (or OpenShift Operator) ?

Use the Kubernetes API to create, configure, and automatically manage applications.

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
