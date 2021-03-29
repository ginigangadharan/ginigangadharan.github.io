---
layout: post
title: Kubernetes Collection
author: gini
categories: [ cloud ]
image: "assets/images/how-to-create-scheduled-snapshots-in-google-cloud-platform.PNG"
tags: [cloud, automation, containers, kubernetes]
permalink: kubernetes
featured: false
hidden: false
titleshort: Kubernetes
---

These are collection of reference documents and blog posts from different experts around.

- [1. Learn Kubernetes](#1-learn-kubernetes)
  - [1.1. Kubernetes Certification](#11-kubernetes-certification)
  - [1.2. CKSS-Certified-Kubernetes-Security-Specialist](#12-ckss-certified-kubernetes-security-specialist)
  - [1.3. Learn Kubernetes from VMWare](#13-learn-kubernetes-from-vmware)
  - [1.4. Learn ECS and EKS](#14-learn-ecs-and-eks)
- [2. Kuberenets ToolBox](#2-kuberenets-toolbox)
  - [Kubernetes Cluster Management Tools](#kubernetes-cluster-management-tools)
  - [2.1. Kubernetes Development Tools](#21-kubernetes-development-tools)
  - [2.2. Kubernetes Security Tools](#22-kubernetes-security-tools)
- [3. kubectl References](#3-kubectl-references)
- [4. Career on Kubernetes](#4-career-on-kubernetes)
  - [4.1. Kubernetes Interview Questions](#41-kubernetes-interview-questions)
- [5. Tanzu Kubernetes Grid - TNG](#5-tanzu-kubernetes-grid---tng)
- [6. References](#6-references)

# 1. Learn Kubernetes 

## 1.1. Kubernetes Certification

- Read my article on Certification & Exam Tips, Learning Paths
  [Certified Kubernetes Administrator (CKA) & Certified Kubernetes Application Developer (CKAD) – Learning Path and Certification](https://www.techbeatly.com/2020/05/kubernetes-certification-cka-ckad-exam-tips-learning-path.html)

- Recommended Course.
  - Certified Kubernetes Administrator (CKA) - [kodekloud](http://bit.ly/ckacourse1) | [Udemy](http://bit.ly/ckacourse2)
  - Certified Kubernetes Application Developer (CKAD) - [Kodekloud](https://bit.ly/ckadcourse2) | [Udemy](https://bit.ly/ckadcourse1)
  
## 1.2. CKSS-Certified-Kubernetes-Security-Specialist

- [CKSS-Certified-Kubernetes-Security-Specialist](https://github.com/ijelliti/CKSS-Certified-Kubernetes-Security-Specialist)
 
## 1.3. Learn Kubernetes from VMWare

| <img src="https://kube.academy/wp-content/themes/k8s/assets/img/logo.svg?t=1588603776" width="200" style="max-width:30vw" alt="KubeAcademy"> | [KubeAcademy](https://kube.academy) |
  
 <br />  

## 1.4. Learn ECS and EKS

- [ECS Workshop by AWS](https://ecsworkshop.com/)
- [AWS CodeDeploy now supports linear and canary deployments for Amazon ECS](https://aws.amazon.com/blogs/containers/aws-codedeploy-now-supports-linear-and-canary-deployments-for-amazon-ecs/)
- [AWS Container Security](https://share-w-partners.s3.amazonaws.com/PartnerTrainingCourses/CT%20-%20Containers%20Technical/NewContainerSecurityVideo_08NOV2019.mp4?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAJYWZKITQ46EGQ5PA/20201111/ap-southeast-1/s3/aws4_request&X-Amz-Date=20201111T035355Z&X-Amz-Expires=3600&X-Amz-SignedHeaders=host&X-Amz-Signature=7143eac1f3222bd078e5b7f313303ee744e9ba1a9a0977fa0b7fc5405687661d)
- [Serverless Workshops](https://github.com/aws-samples/aws-serverless-workshops/)
- [Building Serverless Web Applications with React and AWS Amplify](https://github.com/dabit3/aws-amplify-workshop-react)
- [WILD RYDES](https://webapp.serverlessworkshops.io/)

# 2. Kuberenets ToolBox

## Kubernetes Cluster Management Tools


- **[kubespray](https://github.com/kubernetes-sigs/kubespray)** - Deploy a Production Ready Kubernetes Cluster using Ansible. 
  - **[Deploying Kubernetes with Kubespray](https://www.youtube.com/watch?v=JdgQAsEItTc)** - Video Guide
- **[kubeadm](https://kubernetes.io/docs/reference/setup-tools/kubeadm/)** - Kubeadm is a tool built to provide kubeadm init and kubeadm join as best-practice "fast paths" for creating Kubernetes clusters.
- **[kops](https://github.com/kubernetes/kops)** - kOps - Kubernetes Operations
- **[k9s]([Kubernetes CLI To Manage Your Clusters In Style](https://k9scli.io/))** - K9s is a terminal based UI to interact with your Kubernetes clusters. The aim of this project is to make it easier to navigate, observe and manage your deployed applications in the wild. K9s continually watches Kubernetes for changes and offers subsequent commands to interact with your observed resources.
- ~~**[kube-up.sh](#)**~~ - deprecated.
- **[Cluster API](https://cluster-api.sigs.k8s.io/)** - Cluster API is a Kubernetes sub-project focused on providing declarative APIs and tooling to simplify provisioning, upgrading, and operating multiple Kubernetes clusters.
- **[metalk8s](https://github.com/scality/metalk8s)** - An opinionated Kubernetes distribution with a focus on long-term on-prem deployments
- **[Rancher](https://github.com/rancher/rancher)** - Rancher is an open source project that provides a container management platform built for organizations that deploy containers in production. Rancher makes it easy to run Kubernetes everywhere, meet IT requirements, and empower DevOps teams.
## 2.1. Kubernetes Development Tools

- **[k8slens.dev](https://k8slens.dev/)** - Kuberenetes IDE for developers
- **[portworx.com](https://install.portworx.com)** - Kubernetes spec generator
- **[containerlabs.kubedaily.com](https://containerlabs.kubedaily.com/)**
- **[50+ Useful Kubernetes Tools for 2020](https://caylent.com/50-useful-kubernetes-tools-for-2020)**

## 2.2. Kubernetes Security Tools

- **[kubestriker](https://github.com/vchinnipilli/kubestriker)** - A Blazing fast Security Auditing tool for kubernetes!!
- 
# 3. kubectl References
- [Kubectl Command Cheatsheet](https://www.bluematador.com/learn/kubectl-cheatsheet)
  
# 4. Career on Kubernetes

## 4.1. Kubernetes Interview Questions
- [30 Best Kubernetes Interview Questions and Answers](https://www.whizlabs.com/blog/top-kubernetes-interview-questions/)

# 5. Tanzu Kubernetes Grid - TNG

- Read [VMWare Tanzu](vmware-tanzu)

# 6. References

- [Kubernetes Configuration & Best Practices](https://bcouetil.gitlab.io/academy/BP-kubernetes.html)
- [Kubernetes NodePort vs LoadBalancer vs Ingress? When should I use what?](https://medium.com/google-cloud/kubernetes-nodeport-vs-loadbalancer-vs-ingress-when-should-i-use-what-922f010849e0)

