---
layout: post
title: OpenShift Installation Methods - Examples
author: gini
categories: [ openshift ]
tags: [ OpenShift Installation ]
show-avatar: false
permalink: /openshift-installation
featured: false
hidden: false
titleshort: OpenShift Installation
---

- [Installing an OKD 4.x Cluster](#installing-an-okd-4x-cluster)
- [Install OpenShift 4.x](#install-openshift-4x)
- [Method I - Setup an OpenShift All-In-One](#method-i---setup-an-openshift-all-in-one)
  - [Install required packages](#install-required-packages)
  - [Disable Firewall](#disable-firewall)
  - [Installing OpenShift CLI](#installing-openshift-cli)
    - [Method 1 - Standard CentOS repositories](#method-1---standard-centos-repositories)
    - [Method 2 - Download and extract openshift origin](#method-2---download-and-extract-openshift-origin)
      - [Add the directory you untarred the release into to your path:](#add-the-directory-you-untarred-the-release-into-to-your-path)
  - [Configure the Docker daemon with an insecure registry parameter of 172.30.0.0/16](#configure-the-docker-daemon-with-an-insecure-registry-parameter-of-172300016)
    - [Restart docker service](#restart-docker-service)
  - [Initiate cluster](#initiate-cluster)
    - [Ref:](#ref)
    - [Add a user](#add-a-user)
- [Method II - Setup minishift](#method-ii---setup-minishift)
    - [Setup Virtual Environment](#setup-virtual-environment)
    - [Install minishift](#install-minishift)
    - [Start minishift cluster](#start-minishift-cluster)
- [Method III - OpenShift 4 - OKD - All in One Quick Cluster](#method-iii---openshift-4---okd---all-in-one-quick-cluster)
- [Method IV - OpenShift Full Cluster](#method-iv---openshift-full-cluster)
- [CodeReady Containers - CRC (OpenShift 4.x)](#codeready-containers---crc-openshift-4x)
  - [Download CRC Package](#download-crc-package)
  - [Required software packages](#required-software-packages)
    - [Enable sudo for user](#enable-sudo-for-user)
  - [Setup Cluster](#setup-cluster)
  - [Access your Cluster](#access-your-cluster)
  - [Troubleshooting](#troubleshooting)
- [OpenSHift 4.x](#openshift-4x)
- [OpenShift 4.2 Installation](#openshift-42-installation)
- [Red Hat OpenShift 4.x Installation (Evaluation)](#red-hat-openshift-4x-installation-evaluation)
  - [Baremetal Installation](#baremetal-installation)
- [OpenShift on Baremetal (UPI)](#openshift-on-baremetal-upi)
- [OpenShift All-In-One - OKD Using Ansible](#openshift-all-in-one---okd-using-ansible)
- [Extras OpenSHift 4.x](#extras-openshift-4x)
  - [OpenShift 4.2 Installation](#openshift-42-installation-1)
  - [OpenShift 4.1 Installation](#openshift-41-installation)
  - [Baremetal Installation](#baremetal-installation-1)
- [Install OpenShift 4.x Cluster on VMWare](#install-openshift-4x-cluster-on-vmware)
  - [Install Pre-Req](#install-pre-req)
  - [Download VMWare root CA certificates to System trust](#download-vmware-root-ca-certificates-to-system-trust)
  - [Install Terraform](#install-terraform)
  - [Add VMWare Credential](#add-vmware-credential)
  - [Configure DNS](#configure-dns)
  - [Create `install-config.yaml`](#create-install-configyaml)
  - [Init Cluster](#init-cluster)
  - [Monitoring OpenShift Installations](#monitoring-openshift-installations)
  - [Deleting a Cluster](#deleting-a-cluster)
- [baremetal](#baremetal)
- [Deploy OpenShift Using OpenShift Installer (AWS)](#deploy-openshift-using-openshift-installer-aws)
  - [Using OpenShift Installer](#using-openshift-installer)
  - [Setup Bastion Host](#setup-bastion-host)
    - [Configure Bastion VM to Run OpenShift Installer](#configure-bastion-vm-to-run-openshift-installer)
    - [Install OpenShift Container Platform](#install-openshift-container-platform)
    - [multi-step installation.](#multi-step-installation)
    - [Clean Up Cluster](#clean-up-cluster)
    - [Validate Cluster](#validate-cluster)
- [OpenShift Installation on  Red Hat Virtualization (RHV)/oVirt](#openshift-installation-on--red-hat-virtualization-rhvovirt)
  - [Requirements](#requirements)
- [Troubleshooting](#troubleshooting-1)
- [References](#references)
# Installing an OKD 4.x Cluster

[Reference](https://www.openshift.com/blog/guide-to-installing-an-okd-4-4-cluster-on-your-home-lab)
[Installing an OKD 4.5 Cluster](https://medium.com/@craig_robinson/guide-installing-an-okd-4-5-cluster-508a2631cbee)

# Install OpenShift 4.x

[How to install OpenShift 4 on Bare Metal - (UPI)](https://www.youtube.com/watch?v=d03xg2PKOPg) (Video)

# Method I - Setup an OpenShift All-In-One 

## Install required packages
```
yum install ansible docker wget -y
systemctl enable docker
systemctl start docker
```

## Disable Firewall
Or you have to open required ports.
```
systemctl disable firewalld
systemctl stop firewalld
```

## Installing OpenShift CLI

### Method 1 - Standard CentOS repositories
```
yum -y install centos-release-openshift-origin39
yum -y install origin-clients
```

### Method 2 - Download and extract openshift origin

Create a directory for data (anywhere)
```
mkdir /data
cd /data
```

Check the latest version if you need.
```
wget "https://github.com/openshift/origin/releases/download/v3.11.0/openshift-origin-client-tools-v3.11.0-0cbc58b-linux-64bit.tar.gz"
tar -xzvf openshift-origin-client-tools-v3.11.0-0cbc58b-linux-64bit.tar.gz 
cd openshift-origin-client-tools-v3.11.0-0cbc58b-linux-64bit
```

#### Add the directory you untarred the release into to your path:
```
export PATH=$PATH:/data/openshift-origin-client-tools-v3.11.0-0cbc58b-linux-64bit/

# or

export PATH=$PATH:`pwd`
```

## Configure the Docker daemon with an insecure registry parameter of 172.30.0.0/16
```
cat > /etc/docker/daemon.json <<DELIM
{
   "insecure-registries": [
     "172.30.0.0/16"
   ]
}
DELIM
```

### Restart docker service
```
service docker restart
```

## Initiate cluster
```
oc cluster up --base-dir="/data/clusterup" --public-hostname=<IP>
```
- `--base-dir=BASE_DIR` : Directory on Docker host for cluster up configuration

(oc cluster up --public-hostname=35.239.51.76 --routing-suffix=35.239.51.76.xip.io)
openshift.local.clusterup/openshift-controller-manager/openshift-master.kubeconfig


### Ref:
- https://github.com/openshift/origin/blob/release-3.11/docs/cluster_up_down.md
- https://medium.com/@fabiojose/working-with-oc-cluster-up-a052339ea219


### Add a user
```
# oc create user redhat
user.user.openshift.io/redhat created
# oc adm policy add-cluster-role-to-user cluster-admin redhat
cluster role "cluster-admin" added: "redhat"
```

# Method II - Setup minishift

### Setup Virtual Environment

- setup KVM or virtualbox (or other virtulization)
Ref: [Set up your virtualization environment](https://docs.okd.io/latest/minishift/getting-started/setting-up-virtualization-environment.html)

### Install minishift
- [Download](https://github.com/minishift/minishift/releases) and manually install minishift.
- On mac `brew cask install minishift` (*in case of issue to install, try `export HOMEBREW_NO_ENV_FILTERING=1`*)

### Start minishift cluster

```bash
minishift start --vm-driver virtualbox

## setup VirtualBox Permanently
minishift config set vm-driver virtualbox
```

Once started, access the console using url or open in browser
```bash
minishift console
```

Setup `oc` access
```bash
$ minishift oc-env
export PATH="/home/john/.minishift/cache/oc/v1.5.0:$PATH"

## Run this command to configure your shell:
# eval $(minishift oc-env)
```

# Method III - OpenShift 4 - OKD - All in One Quick Cluster

https://github.com/openshift/okd/releases

# Method IV - OpenShift Full Cluster

- [Installing OpenShift 4.1 Using Libvirt and KVM](http://alesnosek.com/blog/2019/07/08/installing-openshift-4-dot-1-using-libvirt-and-kvm/)

# CodeReady Containers - CRC (OpenShift 4.x)

[Red Hat CodeReady Containers](https://developers.redhat.com/products/codeready-containers)
  
References: 
- [CodeRead Containers - Red Hat OpenShift 4 on your laptop](https://developers.redhat.com/blog/2019/09/05/red-hat-openshift-4-on-your-laptop-introducing-red-hat-codeready-containers/)
- [Install OpenShift Container Platform 4](https://cloud.redhat.com/openshift/install/crc/installer-provisioned?intcmp=7013a000002CtetAAC)
- [CodeReady Containers on Ubuntu](https://labs.consol.de/devops/linux/2019/11/29/codeready-containers-on-ubuntu.html)
- [Trying out CRC - Jeff](https://www.jeffgeerling.com/blog/2019/trying-out-crc-code-ready-containers-run-openshift-4x-locally)
- [RED HAT CODEREADY CONTAINERS](https://access.redhat.com/documentation/en-us/red_hat_codeready_containers/1.13/html-single/getting_started_guide/index)


## Download CRC Package

Refer : **[Install OpenShift on a laptop with CodeReady Containers](https://cloud.redhat.com/openshift/install/crc/installer-provisioned?intcmp=7013a000002CtetAAC)**

- Visit [https://www.openshift.com/try](https://www.openshift.com/try)
- Choose install on Laptop -> https://cloud.redhat.com/openshift/install/crc/installer-provisioned
- Download for your OS choise (Windows10, MacOS, Linux)
- Move package to your machine folder
- extract the package 
  `tar -xf FILENAME.tar.xz`

- Download and keep the pull secret from same location. You need that later during `crc start`

## Required software packages

CodeReady Containers requires the libvirt and NetworkManager packages. 

```bash
yum install NetworkManager
wget https://mirror.openshift.com/pub/openshift-v4/clients/crc/latest/crc-linux-amd64.tar.xz
```

### Enable sudo for user

```
[devops@node1 ~]$ sudo cat /etc/sudoers.d/devops
[sudo] password for devops:
devops ALL=(ALL) NOPASSWD: ALL
```

## Setup Cluster

```bash
$ cd crc-linux-1.11.0-amd64
$ export PATH=/home/devops/crc-linux-1.11.0-amd64:$PATH

## set up your host operating system for the CodeReady Containers virtual machine.
## Use normal user account
$ crc setup

## Start cluster
## if you are on a terminal without GUI, you will have difficulty to copy/paste pull secret content. in that you can mention the pull secret ful
$ crc start -p /path-to/pull-secret
```

## Access your Cluster
```
$ eval $(crc oc-env)
$ oc login -u developer -p developer

## see credentials
$ crc console --credentials
To login as a regular user, run 'oc login -u developer -p developer https://api.crc.testing:6443'.
To login as an admin, run 'oc login -u kubeadmin -p 8rynV-SeYLc-h8Ij7-YPYcz https://api.crc.testing:6443'

## Access Console
$ crc console
Opening the OpenShift Web Console in the default browser...
```

## Troubleshooting

- **Issue** : After crc start and crc console, oc login fails with Internal error occurred: unexpected response: 503 for a while #740

- **Issue** : Unable to connect to console

```
$ crc ip
$ nmcli conn show
```

# OpenSHift 4.x 

https://github.com/openshift/okd

Create `clouds.yaml`
```
clouds:
  ocp4-dev:
    auth:
      auth_url: http://10.6.1.209:35357/
      project_name: ocp4-dev
      username: ocpadmin
      password: ocpadmin
    region_name: RegionOne
``` 
# OpenShift 4.2 Installation
https://docs.openshift.com/container-platform/4.2/installing/installing_bare_metal/installing-bare-metal.html

# Red Hat OpenShift 4.x Installation (Evaluation)
https://access.redhat.com/documentation/en-us/openshift_container_platform/4.1/html/installing/index

- [Get Evaluation](https://www.redhat.com/en/technologies/cloud-computing/openshift/try-it/success)
- [Installation](https://access.redhat.com/documentation/en-us/openshift_container_platform/4.5/html/architecture/architecture-installation#installation-overview_architecture-installation)

## Baremetal Installation
https://access.redhat.com/documentation/en-us/openshift_container_platform/4.1/html/installing/installing-on-bare-metal
https://docs.openshift.com/container-platform/4.1/installing/installing_bare_metal/installing-bare-metal.html
https://blog.openshift.com/openshift-4-bare-metal-install-quickstart/


# OpenShift on Baremetal (UPI)

- [Installing on OpenShift on Baremetal](https://access.redhat.com/documentation/en-us/openshift_container_platform/4.4/html/installing_on_bare_metal/index)

# OpenShift All-In-One - OKD Using Ansible

Ref:
https://github.com/Gepardec/ansible-role-okd
https://galaxy.ansible.com/gepardec/okd
https://computingforgeeks.com/setup-openshift-origin-local-cluster-on-centos/

# Extras OpenSHift 4.x 

https://github.com/openshift/okd

Create `clouds.yaml`
```
clouds:
  ocp4-dev:
    auth:
      auth_url: http://10.6.1.209:35357/
      project_name: ocp4-dev
      username: ocpadmin
      password: ocpadmin
    region_name: RegionOne
``` 

## OpenShift 4.2 Installation
https://docs.openshift.com/container-platform/4.2/installing/installing_bare_metal/installing-bare-metal.html

## OpenShift 4.1 Installation
https://access.redhat.com/documentation/en-us/openshift_container_platform/4.1/html/installing/index

## Baremetal Installation
https://access.redhat.com/documentation/en-us/openshift_container_platform/4.1/html/installing/installing-on-bare-metal
https://docs.openshift.com/container-platform/4.1/installing/installing_bare_metal/installing-bare-metal.html
https://blog.openshift.com/openshift-4-bare-metal-install-quickstart/

https://blog.openshift.com/revamped-openshift-all-in-one-aio-for-labs-and-fun/

# Install OpenShift 4.x Cluster on VMWare
https://labs.consol.de/container/platform/openshift/2020/01/31/ocp43-installation-vmware.html

## Install Pre-Req

```bash
$ sudo yum install wget git vim
```

## Download VMWare root CA certificates to System trust

```bash
$ wget https://vcenter.lab.local/certs/download.zip
$ unzip download.zip
$ sudo cp certs/lin/* /etc/pki/ca-trust/source/anchors/
$ sudo update-ca-trust extract
```

Ref : [Adding vCenter root CA certificates to your system trust](https://docs.openshift.com/container-platform/4.5/installing/installing_vsphere/installing-vsphere-installer-provisioned.html#installation-adding-vcenter-root-certificates_installing-vsphere-installer-provisioned)


## Install Terraform

```bash
$ sudo yum install -y yum-utils
$ sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/RHEL/hashicorp.repo
$ sudo yum -y install terraform
```

Refer [Terraform Installation Methods](https://learn.hashicorp.com/tutorials/terraform/install-cli)

## Add VMWare Credential

```bash
$ cat .config/ocp/vsphere.yaml
vsphere-user: administrator@vsphere.lan
vsphere-password: "123!"
vsphere-server: 192.168.1.100
vsphere-dc: DC1
vsphere-cluster: AZ1
```

## Configure DNS

https://blog.ktz.me/configure-unbound-dns-for-openshift-4/

## Create `install-config.yaml`

```yaml
apiVersion: v1
baseDomain: lab.local
compute:
- hyperthreading: Enabled
  name: worker
  replicas: 0
controlPlane:
  hyperthreading: Enabled
  name: master
  replicas: 3
metadata:
  name: ocp4
platform:
  vsphere:
    vcenter: 10.6.1.198
    username: administrator@vcenter.lab.local
    password: supersecretpassword
    datacenter: DC1
    defaultDatastore: AZ1
fips: false 
pullSecret: 'YOUR_PULL_SECRET'
sshKey: 'YOUR_SSH_PUBKEY'
```

## Init Cluster

```bash
## Creating Installer Configuration File
$ ./openshift-install create install-config --dir=ocp46 --log-level=debug

## Generating Kubernetes Manifests
$ ./openshift-install create manifests --dir=ocp46 --log-level=debug

## Make master nodes schedulable
## edit ocp46/manifests/cluster-scheduler-02-config.yml and make 
$ cat manifests/cluster-scheduler-02-config.yml
apiVersion: config.openshift.io/v1
kind: Scheduler
metadata:
  creationTimestamp: null
  name: cluster
spec:
  mastersSchedulable: true
  policy:
    name: ""
status: {}

## Generating Ignition Configuration Files
$ ./openshift-install create ignition-configs --dir=ocp46 --log-level=debug

## Create the cluster
$ ./openshift-install create cluster --dir=ocp46 --log-level=debug

## NOTE; In a pre-existing infrastructure installation, you cannot use the openshift-install 
## create cluster command to deploy the cluster because you have already installed the cluster nodes. 
## The bootstrap node installation triggers the cluster installation, so execute the following command 
## sequence to monitor the cluster installation:
[user@demo ~]$ openshift-install wait-for bootstrap-complete --dir=ocp46 --log-level=debug
[user@demo ~]$ openshift-install wait-for install-complete --dir=ocp46 --log-level=debug
## openshift-install wait-for commands do not trigger the cluster installation. 
## It is a recommended practice to use them to monitor the cluster installation.
```

## Monitoring OpenShift Installations
```bash
$ export KUBECONFIG=ocp46/auth/kubeconfig
```

## Deleting a Cluster
```bash
## destroy a cluster
$ ./openshift-install destroy cluster --dir=$HOME/ocp46-cluster
```

# baremetal
https://docs.openshift.com/container-platform/4.3/installing/installing_bare_metal/installing-bare-metal.html#cluster-entitlements_installing-bare-metal

# Deploy OpenShift Using OpenShift Installer (AWS)
(27 Jan 2021)
- [OpenShift Installer](https://www.openshift.com/try)
  
Installer configures entire cloud infrastructure:
- VMs
- Load balancers
- Storage
- Networking
- Other resources

## Using OpenShift Installer

```
openshift-install create cluster --dir=$HOME/mycluster
```
Installer prompts for
- SSH public key
- Platform: aws
- Region: Default in AWS is us-east-1
- Base domain: Public route 53 domain, needs to exist prior to installation
- Cluster name: Must be unique within AWS account
- Pull secret: From Get Started with OpenShift as single-line JSON

## Setup Bastion Host
- Create a bastion node and login to the host.

```
$ ssh user@bastion-host
$ echo $GUID
d0ce
```

### Configure Bastion VM to Run OpenShift Installer

**Install AWS CLI**

```
# Download the latest AWS Command Line Interface
curl "https://s3.amazonaws.com/aws-cli/awscli-bundle.zip" -o "awscli-bundle.zip"
unzip awscli-bundle.zip

# Install the AWS CLI into /bin/aws
./awscli-bundle/install -i /usr/local/aws -b /bin/aws

# Validate that the AWS CLI works
aws --version

# Clean up downloaded files
rm -rf /root/awscli-bundle /root/awscli-bundle.zip
```

**Download OpenShift Installation Binary**
```
OCP_VERSION=4.6.4
wget https://mirror.openshift.com/pub/openshift-v4/clients/ocp/${OCP_VERSION}/openshift-install-linux-${OCP_VERSION}.tar.gz
tar zxvf openshift-install-linux-${OCP_VERSION}.tar.gz -C /usr/bin
rm -f openshift-install-linux-${OCP_VERSION}.tar.gz /usr/bin/README.md
chmod +x /usr/bin/openshift-install
```

**Download and Install OC CLI**
```
wget https://mirror.openshift.com/pub/openshift-v4/clients/ocp/${OCP_VERSION}/openshift-client-linux-${OCP_VERSION}.tar.gz
tar zxvf openshift-client-linux-${OCP_VERSION}.tar.gz -C /usr/bin
rm -f openshift-client-linux-${OCP_VERSION}.tar.gz /usr/bin/README.md
chmod +x /usr/bin/oc
```

- Check that the OpenShift installer and CLI are in /usr/bin:

```bash
ls -l /usr/bin/{oc,openshift-install}

# Setup auto-completion
oc completion bash >/etc/bash_completion.d/openshift
```

- logout from `root`

**Configure AWS CLI Credential**
```bash
export AWSKEY=<YOURACCESSKEY>
export AWSSECRETKEY=<YOURSECRETKEY>
export REGION=us-east-2

mkdir $HOME/.aws
cat << EOF >>  $HOME/.aws/credentials
[default]
aws_access_key_id = ${AWSKEY}
aws_secret_access_key = ${AWSSECRETKEY}
region = $REGION
EOF

# test AWS Access
aws sts get-caller-identity
```

- Open https://www.openshift.com/try and select "Try it in the Cloud", Select AWS
- Choose "Installer-Provisioned-Infrastructure"
- Copy the Pull Secret (save to file for copy)

- Create an ssh key pair

```
ssh-keygen -f ~/.ssh/cluster-${GUID}-key -N ''
```

### Install OpenShift Container Platform

- Installer will generate Ignition configs for the bootstrap, master, and worker machines. 
- The process for bootstrapping a cluster looks like the following:
  - The bootstrap machine boots and starts hosting the remote resources required for the master machines to boot.
  - The master machines fetch the remote resources from the bootstrap machine and finish booting.
  - The master machines use the bootstrap node to form an etcd cluster.
  - The bootstrap node starts a temporary Kubernetes control plane using the newly created etcd cluster.
  - The temporary control plane schedules the production control plane to the master machines.
  - The temporary control plane shuts down, yielding to the production control plane.
  - The bootstrap node injects OpenShift-specific components into the newly formed control plane.
  - The installer then tears down the bootstrap node.

- The result of this bootstrapping process is a fully running OpenShift cluster. The cluster will then download and configure the remaining components needed for day-to-day operation, including the creation of worker machines on supported platforms.

**Run OpenShift Installer**

Run the OpenShift installer and answer the prompts:
- Select your Public Key (which you have created earlier)
- Select aws as the Platform.
- Select any Region near you.
- Select `cluster.yourdomain.com` as the Base Domain.
- For the Cluster Name, type cluster-101 (or any other name)
- When prompted, paste the contents of your Pull Secret in JSON format. Do not include any spaces or white characters and make sure it is in one line

```bash
$ openshift-install create cluster --dir $HOME/cluster-101

# Sample answers
? SSH Public Key /home/user/.ssh/cluster-101-key.pub
? Platform aws
INFO Credentials loaded from the "default" profile in file "/home/user/.aws/credentials"
? Region us-east-2 (Ohio)
? Base Domain cluster.yourdomain.com
? Cluster Name cluster-101
? Pull Secret [? for help] ***************************************************************************************************************************************************************

# wait for installer to finish

*********************************************
INFO Creating infrastructure resources...
INFO Waiting up to 20m0s for the Kubernetes API at https://api.cluster-d0ce.d0ce.sandbox1072.opentlc.com:6443... 
INFO API v1.19.0+9f84db3 up
INFO Waiting up to 30m0s for bootstrapping to complete...
INFO Destroying the bootstrap resources...        
INFO Waiting up to 40m0s for the cluster at https://api.cluster-d0ce.d0ce.sandbox1072.opentlc.com:6443 to initialize... 
INFO Waiting up to 10m0s for the openshift-console route to be created... 
INFO Install complete!
INFO To access the cluster as the system:admin user when using 'oc', run 'export KUBECONFIG=/home/gineesh.madapparambath-fujitsu.c/cluster-d0ce/auth/kubeconfig'
INFO Access the OpenShift web-console here: https://console-openshift-console.apps.cluster-d0ce.d0ce.sandbox1072.opentlc.com
INFO Login to the console with user: "kubeadmin", and password: "6TVYn-33gLY-7ZjrA-eqRQE" 
INFO Time elapsed: 37m56s
```

- Make note of the following items from the output of the install command:
  - The location of the kubeconfig file, which is required for setting the KUBECONFIG environment variable and, as suggested, sets the OpenShift user ID to `system:admin`.
  - The kubeadmin user ID and associated password (GEveR-tBVTB-jJUJB-iC9Jn in the example).
  - The password for the kubeadmin user is also written into the auth/kubeadmin-password file.
  - The URL of the web console - (https://console-openshift-console.apps.cluster-<GUID>.sandbox<SANDBOXID>.opentlc.com in the example) and the credentials (again) to log into the web console.

- Refer `${HOME}/cluster-101/.openshift_install.log` for logs and troubleshooting.

### multi-step installation.

```bash
# Create the installation configuration: 
openshift-install create install-config --dir $HOME/cluster-${GUID}.

# Update the generated install-config.yaml file—for example, change the AWS EC2 instance types.

# Create the YAML manifest templates: 
openshift-install create manifests --dir $HOME/cluster-${GUID}
#  Changing the manifest templates is unsupported.

# Create the YAML manifests:
openshift-install create manifests --dir $HOME/cluster-${GUID}
#  Changing the manifests is unsupported.

# Create the Ignition configuration files: 
openshift-install create ignition-configs --dir $HOME/cluster-${GUID}
# Changing the Ignition configuration files is unsupported.

# Install the cluster: 
openshift-install create cluster --dir $HOME/cluster-${GUID}.

# To delete the cluster, use: 
openshift-install destroy cluster --dir $HOME/cluster-${GUID}.

```

### Clean Up Cluster

```bash
openshift-install destroy cluster --dir $HOME/cluster-${GUID}

# Delete all of the files created by the OpenShift installer:
rm -rf $HOME/.kube
rm -rf $HOME/cluster-${GUID}
```

### Validate Cluster

```bash
# Setup CLI
export KUBECONFIG=$HOME/cluster-${GUID}/auth/kubeconfig
echo "export KUBECONFIG=$HOME/cluster-${GUID}/auth/kubeconfig" >>$HOME/.bashrc

$ oc whoami
system:admin

# get console and login with kubeadmin & password from installation log
$ oc whoami --show-console
https://console-openshift-console.apps.cluster-d0ce.d0ce.sandbox1072.opentlc.com
```

# OpenShift Installation on  Red Hat Virtualization (RHV)/oVirt

https://blogs.ovirt.org/2019/01/ovirt-openshift-part-1/

- [Installing a cluster quickly on RHV](https://docs.openshift.com/container-platform/4.5/installing/installing_rhv/installing-rhv-default.html)
- [Installing a cluster on RHV with customizations](https://docs.openshift.com/container-platform/4.7/installing/installing_rhv/installing-rhv-customizations.html)
- [Support Matrix for OpenShift Container Platform IPI Installation on Red Hat Virtualization](https://access.redhat.com/articles/5485861)

## Requirements

**DNS**
1. API DNS - eg: `api.ocp46.ocp4.lab.local`
2. Apps Wildcard - eg: `*.apps.ocp46.ocp4.lab.local`

# Troubleshooting
- [Troubleshooting OpenShift Installation](https://docs.openshift.com/container-platform/4.6/installing/installing_bare_metal_ipi/ipi-install-troubleshooting.html)
- [Troubleshooting installations](https://docs.openshift.com/container-platform/4.6/support/troubleshooting/troubleshooting-installations.html)
# References
- [Installing a cluster quickly on AWS](https://docs.openshift.com/container-platform/4.6/installing/installing_aws/installing-aws-default.html)
- [OpenShift Installer overview on GitHub](https://github.com/openshift/installer/blob/master/docs/user/overview.md)

