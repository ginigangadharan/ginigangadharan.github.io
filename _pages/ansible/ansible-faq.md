---
layout: post
title: Ansible - Frequently Asked Questions
categories: [ ansible ]
tags: [ansible]
show-avatar: false
permalink: ansible-faq
featured: false
hidden: false
titleshort: Ansible FAQ
---
- [What is Ansible](#what-is-ansible)
- [What is Ansible Tower](#what-is-ansible-tower)
- [Why Ansible is different than other automation tools](#why-ansible-is-different-than-other-automation-tools)
- [Can I use Ansible for Auto Remediation ?](#can-i-use-ansible-for-auto-remediation-)

### What is Ansible  

**Ansible** is an open source automation tool with simple automation language (YAML)

### What is Ansible Tower

Ansible Tower is an enterprise framework for controlling, securing and managing your Ansible Automation with UI and RESTful API

### Why Ansible is different than other automation tools

There are other automation tools like Puppet, Chef, Saltstack etc but they all need a agent to be run on managed nodes. Ansible is agent-less; as long as Ansible node have access to managed node via SSH, API etc, Ansible can manage that node.

[Reference](https://www.whizlabs.com/blog/chef-vs-puppet-vs-ansible/)

### Can I use Ansible for Auto Remediation ?

Yes, you can use Ansible for auto-remediation; but keep in mind that someone has to trigger the Ansible job either manually or via some integrated example. 

Eg: 

- Step 1: Your monitoring software (nagios, icinga, zabbix, Prometheus etc) detects an issue on system configuration (like unauthorized sudo user, or low disk space).
- Step 2: Monitoring tool will create a ticket (ServiceNow, Jira, Slack etc)
- Step 3: Monitoring tool will trigger a call to Ansible Job (custmized ) to remediate.
- Step 4: Ansible will run the job on target node and return result to Monitoring/Ticketing Tool.

