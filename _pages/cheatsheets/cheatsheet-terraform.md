---
layout: post
title: Terraform Cheatsheet
author: gini
categories: [ cheatsheets ]
image: "assets/images/2020/jon-moore-bBavss4ZQcA-unsplash-locks.jpg"
tags: [cloud, automation, containers, Terraform]
permalink: cheatsheet-terraform
featured: false
hidden: false
showindex: true
titleshort: Terraform cheatsheet
---

**About Terraform CLI**

Terraform, a tool created by Hashicorp in 2014, written in Go, aims to build, change and version control your infrastructure. This tool have a powerfull and very intuitive Command Line Interface.

- [Installation](#installation)
  - [Install through curl](#install-through-curl)
- [OR install through tfenv: a Terraform version manager](#or-install-through-tfenv-a-terraform-version-manager)
- [First of all, download the tfenv binary and put it in your PATH.](#first-of-all-download-the-tfenv-binary-and-put-it-in-your-path)

## Installation

### Install through curl
```bash
$ curl -O https://releases.hashicorp.com/terraform/0.11.10/terraform_0.11.10_linux_amd64.zip
$ sudo unzip terraform_0.11.10_linux_amd64.zip -d /usr/local/bin/
$ rm terraform_0.11.10_linux_amd64.zip

## OR install through tfenv: a Terraform version manager
## First of all, download the tfenv binary and put it in your PATH.
$ git clone https://github.com/Zordrak/tfenv.git ~/.tfenv
$ echo 'export PATH="$HOME/.tfenv/bin:$PATH"' >> $HOME/bashrc

##Then, you can install desired version of terraform:
$ tfenv install 0.11.10
Usage
Show version
$ terraform --version
Terraform v0.11.10
Init Terraform
$ terraform init
It’s the 
State
Pull remote state in a local copy
$ terraform state pull > terraform.tfstate
Push state in remote backend storage
$ terraform state push
This command is usefull if for example you riginally use a local tf
state and then you de