---
layout: post
title: Nexus Sonatype
author: gini
categories: [ automation, cicd ]
image: "assets/images/2020/jon-moore-bBavss4ZQcA-unsplash-locks.jpg"
tags: [cloud, automation, containers, kubernetes]
permalink: nexus-sonatype
featured: false
hidden: false
showindex: true
#titleshort: jenkins
---



```bash
## Start a Nexus Repo Container
docker pull sonatype/nexus3

docker run -d --name nexus_repo -p 8081:8081 sonatype/nexus3

## wait for message stating Started Sonatype Nexus OSS 3.20.1-01
docker logs nexus_repo -f

## The default username is admin
## get the initial password
docker exec -i nexus_repo cat /nexus-data/admin.password

## access the portal on http://your-ip-addr:8081
```


```bash
## dockerfile for ansible

