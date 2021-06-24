---
layout: post
title: Ansible AWX Installation

# 1. author: gini
categories: [ cloud ]

# 2. image: "assets/images/gini-redhat-cloudevent-2019-2.jpg"
tags: [cloud, automation, containers, kubernetes]
show-avatar: false
permalink: awx-installation
featured: false
hidden: false
titleshort: AWX
---
Ref: 
- [Ansible AWX Project](https://github.com/ansible/awx)
- [Introduction to Ansible AWX Project](https://www.doag.org/formes/pubfiles/10846538/2018-DO-Alexander_Hofstetter-Einfuehrung_in_Ansible_AWX_Project_der_quot_freie_quot_Ansible_Tower-Praesentation.pdf)
- [AWX Command Line Interface](https://github.com/ansible/awx/tree/devel/awxkit/awxkit/cli)
- [3. Install AWX Using Ansible](#3-install-awx-using-ansible)
  - [3.1. Configure AWX Node for Ansible](#31-configure-awx-node-for-ansible)
  - [3.2. Install Pre-Req roles](#32-install-pre-req-roles)
  - [3.3. Install AWX using Roles](#33-install-awx-using-roles)
- [4. Install AWX Manually](#4-install-awx-manually)
  - [4.1. Install epel repo and then install jq](#41-install-epel-repo-and-then-install-jq)
  - [4.2. Install docker-ce related packages](#42-install-docker-ce-related-packages)
  - [4.3. Enable docker-ce repo and install docker engine.](#43-enable-docker-ce-repo-and-install-docker-engine)
  - [4.4. Install latest docker-compose](#44-install-latest-docker-compose)
  - [4.5. Install AWX dependencies](#45-install-awx-dependencies)
  - [4.6. Download Packages](#46-download-packages)
  - [4.7. Disable dockerhub reference in order to build local images.](#47-disable-dockerhub-reference-in-order-to-build-local-images)
  - [4.8. Create a folder in /opt/ to hold awx psql data](#48-create-a-folder-in-opt-to-hold-awx-psql-data)
  - [4.9. Provide psql data path to installer.](#49-provide-psql-data-path-to-installer)
  - [4.10. Enable SSL](#410-enable-ssl)
  - [4.11. Adding Logo](#411-adding-logo)
  - [4.12. Add admin user and password](#412-add-admin-user-and-password)
  - [4.13. Install AWX](#413-install-awx)
- [5. AWX Backup & Restore](#5-awx-backup--restore)
- [6. Migration](#6-migration)
  - [6.1. Upgrade AWX](#61-upgrade-awx)

# 3. Install AWX Using Ansible

You need an ansible machine (just a machine with ansible configured). This is very easy and straight forward as there are roles availalbe for installaing and configuring Ansible AWX. If you want to know the actual steps and items involved in the AWX installation, try manual method.

## 3.1. Configure AWX Node for Ansible

- Create a node (VM in public or private cloud) and make sure you this is configured for Ansible to access. (You can manually do this or just use any roles if multiple nodes). I using a very simple role **[setup_ansible_user](https://galaxy.ansible.com/ginigangadharan/setup_ansible_user)** which will add a user and configure ssh keys. (You can skip this steps if your new VM already configured with ssh keys and sudo access)

Sample content of `setup-ansible.yaml`

```
- hosts: all
  become: true
  vars_files:
  vars:
    remote_user: devops
    remote_user_public_key: 'YOUR_PUBLIC_KEY' # use your public key to add to remote node
  roles:
    - { role: setup-ansible-user }
```

- Add your AWX node to Inventory

```
[awx]
awx-node-01 ansible_host=YOUR_VM_IP
```

Remember to use proper switches to ask password, become sudo, sudo password etc.

```
ansible-playbook setup-ansible.yaml -b -K -k -u root
```

## 3.2. Install Pre-Req roles

We can use the [`awx`](https://galaxy.ansible.com/geerlingguy/awx) role by [Jeff Geerling](https://galaxy.ansible.com/geerlingguy). But, make sure you have installed all pre-req roles from Ansible Galaxy; add roles in `requirements.yml` and install.

```
$ cat requirements.yml
# from galaxy
- src: geerlingguy.repo-epel
- src: geerlingguy.git
- src: geerlingguy.ansible
- src: geerlingguy.docker
- src: geerlingguy.pip
- src: geerlingguy.nodejs
- src: geerlingguy.awx
```

Then,

```
$ ansible-galaxy install -r requirements.yml

# verify roles directory
$ ls -l roles/
total 32
drwxr-xr-x 6 net_gini net_gini 4096 Feb 12 22:09 geerlingguy.ansible
drwxr-xr-x 7 net_gini net_gini 4096 Feb 12 22:09 geerlingguy.awx
drwxr-xr-x 8 net_gini net_gini 4096 Feb 12 22:09 geerlingguy.docker
drwxr-xr-x 7 net_gini net_gini 4096 Feb 12 22:09 geerlingguy.git
drwxr-xr-x 8 net_gini net_gini 4096 Feb 12 22:09 geerlingguy.nodejs
drwxr-xr-x 6 net_gini net_gini 4096 Feb 12 22:09 geerlingguy.pip
drwxr-xr-x 6 net_gini net_gini 4096 Feb 12 22:08 geerlingguy.repo-epel
drwxr-xr-x 9 net_gini net_gini 4096 Feb 11 15:15 makarenalabs.wordpress
```

## 3.3. Install AWX using Roles

Now, just call your awx install playbook; sample playbook below.

*Note : You may customize AWX installation, refer [`awx`](https://galaxy.ansible.com/geerlingguy/awx) for more.

```
- hosts: awx-node-01
  become: true

  vars:
    nodejs_version: "6.x"
    pip_install_packages:
      - name: docker-py

  roles:
    - geerlingguy.repo-epel
    - geerlingguy.git
    - geerlingguy.ansible
    - geerlingguy.docker
    - geerlingguy.pip
    - geerlingguy.nodejs
    - geerlingguy.awx
```

Install Ansible AWX

```
$ ansible-playbook awx-install.yaml 
```

After AWX is installed, you can log in with the default username `admin` and password `password`.

# 4. Install AWX Manually

## 4.1. Install epel repo and then install jq
```
yum install -y epel-release -y && yum install jq
```

## 4.2. Install docker-ce related packages
```
yum install -y yum-utils device-mapper-persistent-data lvm2
```

## 4.3. Enable docker-ce repo and install docker engine.
```
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
yum -y install docker-ce
systemctl enable docker && systemctl start docker
```

## 4.4. Install latest docker-compose
```
LATEST_VERSION=$(curl -s https://api.github.com/repos/docker/compose/releases/latest | jq -r '.tag_name')
curl -L "https://github.com/docker/compose/releases/download/$LATEST_VERSION/docker-compose-$(uname -s)-$(uname -m)" > /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
```

## 4.5. Install AWX dependencies

```
yum install -y python2-pip
pip install ansible
pip install docker-compose
```

## 4.6. Download Packages

Change dir to the home directory and get ansible-awx release tarball and extract it. 

```
cd ~
curl \
  -o ansible-awx-6.1.0.tar.gz https://codeload.github.com/ansible/awx/tar.gz/6.1.0 && \
  tar xvfz ansible-awx-6.1.0.tar.gz && \
  rm -f ansible-awx-6.1.0.tar.gz
```

## 4.7. Disable dockerhub reference in order to build local images.
```
cd awx-6.1.0
sed -i "s|^dockerhub_base=ansible|#dockerhub_base=ansible|g" installer/inventory
```

## 4.8. Create a folder in /opt/ to hold awx psql data

```
mkdir -p /opt/awx-psql-data
```

## 4.9. Provide psql data path to installer.

```
sed -i "s|^postgres_data_dir.*|postgres_data_dir=/opt/awx-psql-data|g" installer/inventory
```

*Note: If you wish to use an external database, in the inventory file, set the value of pg_hostname, and update pg_username, pg_password, pg_database, and pg_port with the connection information.*


## 4.10. Enable SSL

```
# Create awx-ssl folder in /etc.
mkdir -p /etc/awx-ssl/

# Make a self-signed ssl certificate
openssl req -subj '/CN=labs.local/O=Labs Local/C=TR' \
	-new -newkey rsa:2048 \
	-sha256 -days 1365 \
	-nodes -x509 \
	-keyout /etc/awx-ssl/awx.key \
	-out /etc/awx-ssl/awx.crt


# Merge awx.key and awx.crt files
cat /etc/awx-ssl/awx.key /etc/awx-ssl/awx.crt > /etc/awx-ssl/awx-bundled-key.crt

# Pass the full path of awx-bundled-key.crt file to ssl_certificate variable in inventory.
sed -i -E "s|^#([[:space:]]?)ssl_certificate=|ssl_certificate=/etc/awx-ssl/awx-bundled-key.crt|g" installer/inventory
```

## 4.11. Adding Logo

```
# Change dir to where awx main folder is placed:
cd ~

# Download and extract awx-logos repository. 
curl -L -o awx-logos.tar.gz https://github.com/ansible/awx-logos/archive/master.tar.gz
tar xvfz awx-logos.tar.gz

# Rename awx-logos-master folder as awx-logos  
mv awx-logos-master awx-logos

# Remove tarball
rm -f *awx*.tar.gz
```

*Note: AWX installer which resides at `installer/install.yml`, searches for `awx-logos` directory as `../../awx-logos`. So, your `awx-logos` folder should be located at where awx installer’s parent directory is placed.*

```
# Change dir to awx and replace awx_official parameter
cd ~/awx-6.1.0
sed -i -E "s|^#([[:space:]]?)awx_official=false|awx_official=true|g" installer/inventory
```

## 4.12. Add admin user and password

```
# Define the default admin username
sed -i "s|^admin_user=.*|admin_user=awx-admin|g" installer/inventory

# Set a password for the admin
sed -i "s|^admin_password=.*|admin_password=CHANGE_ME|g" installer/inventory
```

## 4.13. Install AWX

```
# Enter the installer directory.
cd ~/awx-6.1.0/installer

# Initiate install.yml
ansible-playbook -i inventory install.yml
```

Reference:
- [Installing AWX](https://github.com/ansible/awx/blob/devel/INSTALL.md)
- [Ansible AWX Installatio](https://medium.com/swlh/ansible-awx-installation-5861b115455a)


# 5. AWX Backup & Restore

http://yallalabs.com/automation-tool/how-to-backup-restore-awx-ansible-tower-objects-tower-cli-tool/
https://www.unixarena.com/2019/03/backup-restore-ansible-awx-tower-cli.html/
https://www.insentragroup.com/au/insights/geek-speak/modern-workplace/protecting-the-automation-engine-backup-for-ansible-awx-project/


# 6. Migration

- [Migrating Data Between AWX Installations](https://github.com/ansible/awx/blob/devel/DATA_MIGRATION.md)
- [Upgrading from previous versions](https://github.com/ansible/awx/blob/devel/INSTALL.md#upgrading-from-previous-versions)

## 6.1. Upgrade AWX
https://stackoverflow.com/questions/59624053/upgrade-ansible-tower-minor-upgrade/59639499#59639499

If you used the docker-compose installation method and pointed postgres_data_dir to a persistent directory on the host, upgrading AWX is straightforward. I deployed AWX 2.0.0 in 2018 and have upgraded it to every subsequent release (currently running 9.1.0) without issue. Below is my upgrade method which preserves all data including secrets between upgrades and does not rely on using the tower cli / awx cli tool.

AWX path assumptions:

Existing installation: /opt/awx

New release: /tmp/awx

AWX inventory file assumptions:

use_docker_compose=true
postgres_data_dir=/opt/postgres
docker_compose_dir=/var/lib/awx
Manual upgrade process:

Backup your AWX host before continuing! Consider backing up your postgres database as well.
Download the new release of AWX and unpack it to /tmp/awx
Ensure that the patch package is installed on the host.
Create a patch file containing the differences between the new and existing inventory files:
diff -u /tmp/awx/installer/inventory /opt/awx/installer/inventory > /tmp/awx_inv_patch

Patch the new inventory file with the differences:
patch /tmp/awx/installer/inventory < /tmp/awx_inv_patch

Verify that the files now match:
diff -s /tmp/awx/installer/inventory /opt/awx/installer/inventory

Copy the new release directory over the existing one:
cp -Rp /tmp/awx/* /opt/awx/

Edit /var/lib/awx/docker-compose.yml and change the version numbers after image: ansible/awx_web: and image: ansible/awx_task: to match the new version of AWX that you're upgrading to.
Stop the current AWX containers:
cd /var/lib/awx

docker-compose stop

Run the installer:
cd /opt/awx/inventory

ansible-playbook -i inventory install.yml

AWX starts the upgrade process, which usually completes within a couple minutes. I'll typically monitor the upgrade progress with docker logs -f awx_web until I see RESULT 2 / OKREADY appear.

If everything is working as intended, I shut the containers down, pull and then recreate them using docker-compose:
cd /var/lib/awx

docker-compose stop

docker-compose pull && docker-compose up --force-recreate -d

If everything is still working as intended, I delete /tmp/awx and /tmp/awx_inv_patch.