---
layout: post
title: Ansible for Infoblox
author: gini
categories: [ ansible ]
image: "assets/images/2020/photo-1456428746267-a1756408f782-@clintadair.jpg"
tags: [ansible, infoblox]
permalink: ansible-for-infoblox
featured: false
hidden: false
titleshort: infoblox
---

- [Configure Ansible for Infoblox](#configure-ansible-for-infoblox)
  - [Install Python3 on RHEL](#install-python3-on-rhel)
  - [Instal Python3 on Centos](#instal-python3-on-centos)
  - [Setup Python VirtualEnv (venv)](#setup-python-virtualenv-venv)
  - [DONOT Upgrade pip to Latest !!!](#donot-upgrade-pip-to-latest-)
  - [Install Ansible](#install-ansible)
  - [Install infoblox Client](#install-infoblox-client)
- [Infoblox API References](#infoblox-api-references)
    - [Sample API Calls](#sample-api-calls)
- [Appendix](#appendix)
- [About Infoblox vNIOS Virtual Appliance for Microsoft Hyper-V](#about-infoblox-vnios-virtual-appliance-for-microsoft-hyper-v)

# Configure Ansible for Infoblox
## Install Python3 on RHEL

https://developers.redhat.com/blog/2018/08/13/install-python3-rhel/#why-rhscl

```bash
$ su -
# subscription-manager repos --enable rhel-7-server-optional-rpms \
  --enable rhel-server-rhscl-7-rpms
# yum -y install @development
# yum -y install rh-python36
 
# yum -y install rh-python36-numpy \
 rh-python36-scipy \ 
 rh-python36-python-tools \
 rh-python36-python-six
```

## Instal Python3 on Centos

```bash
# yum install -y python3
```

## Setup Python VirtualEnv (venv)

```bash
$ scl enable rh-python36 bash
$ python3 -V
Python 3.6.3
 
$ python -V  # python now also points to Python3 
Python 3.6.3
 
$ mkdir ~/pydev
$ cd ~/pydev
 
$ python3 -m venv py36-venv
$ source py36-venv/bin/activate
 
(py36-venv) $ python3 -m pip install ...some modules...
```

## DONOT Upgrade pip to Latest !!!
```
$ python -m pip install pip==20.2.3
```

## Install Ansible

```bash
## install required packages
pip install wheel setuptools rust etc

pip install ansible
```

## Install infoblox Client

```bash
pip install infoblox-client
```

# Infoblox API References

Find the WAPI documentation from `https://<GRID_IP_ADDRESS>/wapidoc/` url.

```yaml
## list WAPI versions
https://10.10.10.10/wapi/v1.0/?_schema

## samples
https://10.6.1.216/wapi/v2.7/networkview?name=default
https://10.6.1.216/wapi/v2.7/networkview?name=default&_return_type=json
```

### Sample API Calls

```bash
$ curl -k -s -H 'content-type: application/json' -X POST --user admin:adminadmin "https://10.6.1.216/wapi/v2.7/record:rpz:a?_return_fields%2B=name,rp_zone&_return_as_object=1" -d '{"name":"serverblock.com.category-01","ipv4addr":"1.1.1.156","rp_zone":"category-01"}'

```

# Appendix

- [Infoblox Guide](https://docs.ansible.com/ansible/latest/scenario_guides/guide_infoblox.html)
- [Automate Infoblox Infrastructure Using Ansible](https://www.infoblox.com/wp-content/uploads/infoblox-deployment-guide-automate-infoblox-infrastructure-using-ansible.pdf)
- [Ansible and Infoblox: Roles Deep Dive](https://www.ansible.com/blog/ansible-and-infoblox-roles-deep-dive)
- [Infoblox and Ansible Integration](https://www.infoblox.com/wp-content/uploads/infoblox-deployment-guide-infoblox-and-ansible-integration.pdf)
# About Infoblox vNIOS Virtual Appliance for Microsoft Hyper-V

- [Installing vNIOS Virtual Appliance](https://docs.infoblox.com/display/NMHIG/Installing+vNIOS+Virtual+Appliance)


- [The definitive list of REST examples](https://community.infoblox.com/t5/API-Integration/The-definitive-list-of-REST-examples/td-p/1214)