---
layout: post
title: Ansible - Learning Docs & References
categories: [ cloud ]
image: "assets/images/2020/ansible-automation.png"
tags: [cloud, automation, containers, kubernetes]
permalink: /ansible
featured: false
hidden: false
titleshort: Ansible
---

# Ansible for Absolute Beginners

**[Automation with Ansible – All You Want to Learn](https://www.techbeatly.com/ansible/){:target="_blank"}** 

You have hundreds and thousands of online documentation and wiki pages for learning Ansible, and this is my attempt to cover Ansible in a simple and beginner level approach. Access all chapters on **[techbeatly.com](https://www.techbeatly.com/ansible/){:target="_blank"}**. 

- **[Ansible – Part 1 – Introduction](https://www.techbeatly.com/2018/06/ansible-part-1-introduction.html/){:target="_blank"}**
- **[Ansible – Part 2 – Installing Ansible](https://www.techbeatly.com/2018/06/ansible-part-2-installing-ansible.html/){:target="_blank"}**
- **[Ansible – Part 3 – Deployment](https://www.techbeatly.com/2018/06/ansible-part-3-deployment.html/){:target="_blank"}**
- **[Ansible – Part 4 – Managing Inventory](https://www.techbeatly.com/2018/06/ansible-part-4-managing-inventory.html/){:target="_blank"}**
- **[Ansible – Part 5 – Running Ad-Hoc commands](https://www.techbeatly.com/2018/06/ansible-part-5-running-ad-hoc-commands.html/){:target="_blank"}**
- **[Ansible – Part 6 – Play with Playbooks](https://www.techbeatly.com/2018/06/ansible-part-6-play-with-playbooks.html/){:target="_blank"}**
- **[Ansible – Part 7 – Managing Variables](https://www.techbeatly.com/2019/08/ansible-part-7-managing-variables.html/){:target="_blank"}**
- **[Ansible – Part 8 – Managing Facts](https://www.techbeatly.com/2019/09/ansible-part-8-managing-facts.html/){:target="_blank"}**
- *(Ansible – Part 9 – Task Control <In Progress … >)*
- *(Ansible – Part 10 – Jinja2 Templates with Ansible <In Progress … >)*
- **[Ansible – Part 11 – Implementing Roles](https://www.techbeatly.com/2018/06/ansible-part-11-implementing-roles.html/){:target="_blank"}**
- **[Ansible – Part 12 – Deploying Roles With Ansible Galaxy](https://www.techbeatly.com/2018/06/ansible-part-12-deploying-roles-with-ansible-galaxy.html/){:target="_blank"}**
- *(More Chapters are coming, give me some more time to prepare)*

<h3>Table of Contents</h3>

- [Ansible for Absolute Beginners](#ansible-for-absolute-beginners)
- [Ansible Videos for Beginners](#ansible-videos-for-beginners)
- [Ansible - Frequently Asked Questions](#ansible---frequently-asked-questions)
- [Ansible Playbook References](#ansible-playbook-references)
- [Tools for Ansible](#tools-for-ansible)
- [Ansible for Network Automation](#ansible-for-network-automation)
- [Ansible Tower](#ansible-tower)
- [Ansible and Python Modules](#ansible-and-python-modules)
  - [Ansible and Python 3](#ansible-and-python-3)
- [Ansible Blog/Articles to Follow](#ansible-blogarticles-to-follow)
- [Ansible Interview Questions](#ansible-interview-questions)
- [Ansible Modules](#ansible-modules)
- [Other References](#other-references)
- [Ansible for IBM Power](#ansible-for-ibm-power)
- [Ansible for Arista Networks](#ansible-for-arista-networks)
- [Ansible for CIS Hardening/CIS Check](#ansible-for-cis-hardeningcis-check)

# Ansible Videos for Beginners

- [Introduction to Ansible - Webinar](https://www.youtube.com/watch?v=iVWmbStE1MM){:target="_blank"} (YouTube)
- [Ansible - A Beginner's Tutorial](https://www.youtube.com/watch?v=icR-df2Olm8){:target="_blank"} (YouTube)

# Ansible - Frequently Asked Questions

1.	What is Ansible?
2.	What is IaC? 
3.	What can Ansible do?
4.	What are the advantages of Ansible?
5.	How does Ansible work?
6.	What is a Playbook?
7.	Are there any requirements for using Ansible?
8.	What is DevOps?
9.	How does Ansible fit into DevOps?
10.	Who is Ansible for? Who should learn Ansible?
11.	What are prerequisites to learning Ansible?

# Ansible Playbook References

- [Ansible Examples](https://github.com/ginigangadharan/ansible-examples){:target="_blank"}
(*Forked from [@ansible](https://github.com/ansible/ansible-examples)*)
- [Ansible for DevOps](https://github.com/ginigangadharan/ansible-for-devops){:target="_blank"} 
(*Forked from [geerlingguy](https://github.com/geerlingguy/ansible-for-devops)*)
- [Ansible NXOS Samples](https://github.com/ginigangadharan/Ansible-NXOS){:target="_blank"} (Network Automation)

# Tools for Ansible

- [Ansible Silo](https://groupon.github.io/ansible-silo){:target="_blank"}

# Ansible for Network Automation

- [Network Device Authentication with Ansible 2.3](https://www.ansible.com/blog/network-device-authentication-with-ansible-2-3){:target="_blank"}

# Ansible Tower

- [What Happens to an Ansible Tower Installation After the Subscription/License has Expired?](https://access.redhat.com/solutions/3389421)

# Ansible and Python Modules
[How to install pip on Red Hat Enterprise Linux?](https://access.redhat.com/solutions/1519803)


pip install --target=/your/pyinstalldir loremipsum
pip install fortiosapi --target=/var/lib/awx/venv/ansible/lib/python2.7/site-packages

pip list --target=/var/lib/awx/venv/ansible/lib/python2.7/site-packages

[Running in a virtualenv](https://docs.ansible.com/ansible/latest/reference_appendices/faq.html#running-in-a-virtualenv)

```
# subscription-manager repos --enable rhel-server-rhscl-7-rpms
# yum install python27-python-pip
```

Switch to a normal user and check the pip
```
$ scl enable python27 bash
$ which pip
$ pip -V
```

**For Existing Tower**

```
# source /var/lib/awx/venv/ansible/bin/activate
# umask 0022
# pip install --upgrade pywinrm
# deactivate
```

## Ansible and Python 3

- [Python 3 Support](https://docs.ansible.com/ansible/latest/reference_appendices/python_3_support.html)
- [How Can I Use Virtual Environment with Python3 on RHEL 7 for Ansible Tower?](https://access.redhat.com/solutions/4371201)
- [How do I use Python 3 in Ansible Tower?](https://access.redhat.com/solutions/3569661)
- [Ansible and Python 3](https://docs.ansible.com/ansible/latest/dev_guide/developing_python_3.html#ansible-and-python-3) (docs.ansible.com/)

Ansible 2.5 and above work with Python 3.

```
[ansible@vm-ans-02 ~]$ ansible --version |grep python
  ansible python module location = /usr/lib/python2.7/site-packages/ansible
  python version = 2.7.5 (default, Sep 26 2019, 13:23:47) [GCC 4.8.5 20150623 (Red Hat 4.8.5-39)]
```


# Ansible Blog/Articles to Follow
- [MyDailyTutorials](http://www.mydailytutorials.com/category/tutorials/ansible/){:target="_blank"}
- [Ansible Blog](https://www.ansible.com/blog){:target="_blank"}
- [Ansible & Cisco](https://blogs.cisco.com/tag/ansible){:target="_blank"}
- [ansiblejunky.com](https://www.ansiblejunky.com/){:target="_blank"}
- [ansible linting - Abhijeet Kamble](https://medium.com/faun/linting-your-ansible-playbooks-and-make-a-continuous-integration-ci-solution-bcf8b4ea4c03){:target="_blank"}

# Ansible Interview Questions
- [Question Bank 1](https://career.guru99.com/ansible-interview-questions-answers/){:target="_blank"}
- [Question Bank 2](https://www.edureka.co/blog/interview-questions/ansible-interview-questions/){:target="_blank"}
- [Question Bank 3](https://mindmajix.com/ansible-interview-questions){:target="_blank"}

# Ansible Modules

- [Foreman Ansible Modules](https://github.com/theforeman/foreman-ansible-modules)

# Other References

- [Red Hat Ansible Tower Life Cycle](https://access.redhat.com/support/policy/updates/ansible-tower)
- [Red Hat Ansible Automation Platform Workshops](https://github.com/ansible/workshops)
- [DEEP DIVE INTO ANSIBLE NETWORK RESOURCE MODULE](https://www.ansible.com/deep-dive-into-ansible-network-resource-module)

# Ansible for IBM Power

- [IBM Power Systems AIX collection](https://github.com/IBM/ansible-power-aix)

# Ansible for Arista Networks

- [Ansible Modules for Arista CloudVision Platform](https://github.com/aristanetworks/ansible-cvp)

# Ansible for CIS Hardening/CIS Check
- [DevSec Hardening Framework](https://github.com/dev-sec) / [devops + security - Server Hardening Automation](https://dev-sec.io/)