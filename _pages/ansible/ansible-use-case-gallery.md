---
layout: post
title: Ansible Use Case Gallery
author: gini
categories: [ ansible ]
image: "assets/images/2021/lenny-kuhne-jHZ70nRk7Ns-unsplash -automation.jpg"
tags: []
show-avatar: false
permalink: /ansible-use-case-gallery
featured: false
hidden: false
titleshort: Ansible Use Case
---

- [Immutable Infrastructure](#immutable-infrastructure)
- [Provisioning](#provisioning)
  - [AWS](#aws)
- [Application Deployment](#application-deployment)
- [Security Management](#security-management)
- [Manage network configurations](#manage-network-configurations)
- [Weekly system reboot](#weekly-system-reboot)
- [Enforce security guidelines](#enforce-security-guidelines)
- [Configuration Management](#configuration-management)
- [Disaster recovery](#disaster-recovery)
- [Adhoc Commands](#adhoc-commands)
- [Database](#database)
  - [Ansible Oracles Modules](#ansible-oracles-modules)
  - [Database binary patching](#database-binary-patching)
- [Service license agreements](#service-license-agreements)
  
## Immutable Infrastructure

- Configure your re-buildable replicas of server set or application stack.
- Use Ansible for **Infrastructure as Code** (IaC)

## Provisioning

Provision your bare-metal, private cloud and public cloud infrastructure using modules and configure them to use for day 2 operations.

### AWS 

- Instance Provisioning
- LoadBalancer /  TargetGroup Creation
- VPC Creation

## Application Deployment

Manage your applications deployments, re-deployments, migration using simple ansible playbooks; and implement DevSecOps methods in your infrastructure operations.

## Security Management

- Incident Response
- 

## Manage network configurations

- Configure new network devices with same configurations as we need
- Add new rules or changes in configurations (eg: new VLAN, new Virtual Interface)
- Add/Remove rule in firewall

## Weekly system reboot
Eliminate repetitive, manual processes with automation.

## Enforce security guidelines
Rules are rules. It’s best to automate in an effort to achieve strict security standards.

## Configuration Management

Keep your configurations as a code and also can check if someone made chages to the same.

## Disaster recovery
Disaster recovery can involve a wide range of components. Act across different variables of the technology stack to identify problems and eliminate cross team dependencies.

## Adhoc Commands
Remarkably easy to write, you can run commands across your environment for any number of servers.

## Database

### Ansible Oracles Modules
- [ansible-oracle-modules](https://github.com/oravirt/ansible-oracle-modules)

### Database binary patching
Several databases use outdated binary sets. Patch the binaries in accordance with the release of the latest patch.

## Service license agreements
Mistakes cost time and money. Eliminate errors that can crop up in detailed software contracts.