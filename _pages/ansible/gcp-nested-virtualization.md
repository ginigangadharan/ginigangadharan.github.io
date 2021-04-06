---
layout: post
title: GCP Nested Virtualization
author: gini
categories: [ ansible ]
#image: "assets/images/2020/photo-1456428746267-a1756408f782-@clintadair.jpg"
tags: [ansible, infoblox]
permalink: gcp-nested-virtualization
featured: false
hidden: false
#titleshort: infoblox
---

*Note: This is purely based on [GCP Documentation](https://cloud.google.com/compute/docs/instances/enable-nested-virtualization-vm-instances) and for my own quick reference with additonal notes.*

```bash
## Create a boot disk from a public image 
$ gcloud compute disks create disk1 --image-project debian-cloud --image-family debian-9 --zone us-central1-b


## Create image
$ gcloud compute images create nested-vm-image \
  --source-disk disk1 --source-disk-zone us-central1-b \
  --licenses "https://www.googleapis.com/compute/v1/projects/vm-options/global/licenses/enable-vmx"

## remove the disk since it not needed anymore

## Create VM with new Image
$ gcloud compute instances create example-nested-vm --zone us-central1-b \
              --min-cpu-platform "Intel Haswell" \
              --image nested-vm-image

## SSH Access to the VM
$ gcloud compute ssh example-nested-vm
## and check the nested virtualization inside VM
$ grep -cw vmx /proc/cpuinfo
```

**Starting a nested VM**

```bash
## Update the VM instance and install some necessary packages
$ sudo apt update && sudo apt install qemu-kvm -y

## Download OS Image
$ wget https://people.debian.org/~aurel32/qemu/amd64/debian_squeeze_amd64_standard.qcow2

## Run inside screen and press enter
$ screen

## SStart the nested VM. When prompted, login in with user: root, password: root
sudo qemu-system-x86_64 -enable-kvm -hda debian_squeeze_amd64_standard.qcow2 -m 512 -curses
```

**Starting a private bridge between the host and nested VMs**

```bash
## insall necessary pacakges
$ sudo apt update && sudo apt install uml-utilities qemu-kvm bridge-utils virtinst libvirt-daemon-system libvirt-clients -y

## Start the default network that comes with the libvirt package:
$ sudo virsh net-start default
```


## Appendix

### Install GNOME on Debian

```bash
$ sudo tasksel install desktop gnome-desktop
$ sudo tasksel install laptop

## To tell Debian 10 to start the graphical desktop environment by default on boot, run the following command:
$ sudo systemctl set-default graphical.target
$ sudo reboot
```