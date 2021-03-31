---
layout: post
title: Jenkins
author: gini
categories: [ automation, cicd ]
image: "assets/images/2020/jon-moore-bBavss4ZQcA-unsplash-locks.jpg"
tags: [cloud, automation, containers, kubernetes]
permalink: jenkins
featured: false
hidden: false
showindex: true
titleshort: jenkins
---

- [Pipeline Introduction](#pipeline-introduction)
  - [PROJECT TYPES](#project-types)
  - [JENKINS PIPELINE](#jenkins-pipeline)
  - [PIPELINE BENEFITS (1/2)](#pipeline-benefits-12)
  - [PIPELINE-AS-CODE](#pipeline-as-code)
  - [Basic Jenkins Pipeline Sections](#basic-jenkins-pipeline-sections)
  - [Jenkins Parallel Pipeline](#jenkins-parallel-pipeline)
  - [SCRIPTED PIPELINE](#scripted-pipeline)
  - [Multi-environment Pipeline](#multi-environment-pipeline)
  - [Post Section in Jenkins Pipeline](#post-section-in-jenkins-pipeline)
  - [Environment Directive](#environment-directive)
  - [Notifications](#notifications)
    - [Conditional Notificaton](#conditional-notificaton)
  - [WHEN DIRECTIVE](#when-directive)
  - [GIT ENVIRONMENT VARIABLES](#git-environment-variables)
- [Start Jenkins inside a container](#start-jenkins-inside-a-container)
  - [Run `jenkins` as a docker container](#run-jenkins-as-a-docker-container)
  - [Get the Password](#get-the-password)
  - [Login to Jenkins GUI for the first time](#login-to-jenkins-gui-for-the-first-time)
  - [Sample docker-compose.yml](#sample-docker-composeyml)
- [Create Multibranch Pipeline with Git](#create-multibranch-pipeline-with-git)
- [Learn Jenkins](#learn-jenkins)
- [Reference](#reference)


# Pipeline Introduction

## PROJECT TYPES
The Continuous Delivery build flow is defined as a project, of one of the following types:

- Freestyle Projects 
- Pipeline Projects
- Declarative Pipeline
- Scripted Pipeline

## JENKINS PIPELINE
https://jenkins.io/doc/book/pipeline/

## PIPELINE BENEFITS (1/2)
The pipeline functionality is:

- Durable: The Jenkins master can restart and the Pipeline continues to run
- Pausable: can stop and wait for human input or approval
- Versatile: supports complex real-world CD requirements (fork, join, loop, parallelize)
- Extensible: supports custom extensions to its "DSL" (Domain-specific Language)
- Reduces number of jobs
- Easier maintenance
- Decentralization of job configuration
- Easier specification through code

## PIPELINE-AS-CODE

- A Pipeline is defined in a Jenkinsfile - Uses a DSL based on Apache Groovy syntax
- Deployment flow is expressed as code - Can express complex flows, conditionals and such
- The Jenkinsfile is stored on an SCM - Works with SCM conventions such as Branches and Git Pull Requests

**Jenkins Componenets**
- Master: Computer, VM or container where Jenkins is installed and run. Serves requests and handles build tasks
- Agent: (formerly "slave") Computer, VM or container that connects to a Jenkins Master. Executes tasks when - directed by the Master. Has a number and scope of operations to perform.
- Node:  is sometimes used to refer to the computer, VM or container used for the Master or Agent; be careful because "Node" has another meaning for Pipeline
- Executor: Computational resource for running builds. Performs Operations, Can run on any Master or Agent, although running builds on masters, can degrade performance and opens up serious security vulnerabilities, Can be parallelized on a specific Master or Agent

## Basic Jenkins Pipeline Sections

```yaml
pipeline {
  agent { label 'linux' }
  stages {
    stage('MyBuild') {
      steps {
        sh './jenkins/build.sh'
      }
    } 
    stage('MySmalltest') {
      steps {
        sh './jenkins/smalltest.sh'
      }
    }
  }
}
```

## Jenkins Parallel Pipeline

```yaml
pipeline {
  agent any
  stages {
    stage('Fluffy Build') {
      steps {
        sh './jenkins/build.sh'
        archiveArtifacts 'target/*.jar'
      }
    }
    stage('Fluffy Test') {
      parallel {
        stage('Backend') {
          steps {
            sh './jenkins/test-backend.sh'
            junit 'target/surefire-reports/**/TEST*.xml'
          }
        }
        stage('Frontend') {
          steps {
            sh './jenkins/test-frontend.sh'
            junit 'target/test-results/**/TEST*.xml'
          }
        }
        stage('Performance') {
          steps {
            sh './jenkins/test-performance.sh'
          }
        }
        stage('Static') {
          steps {
            sh './jenkins/test-static.sh'
          }
        }
      }
    }
    stage('Fluffy Deploy') {
      steps {
        sh './jenkins/deploy.sh staging'
      }
    }
  }
}
```

## SCRIPTED PIPELINE

```yaml
stage('Build') {
    parallel linux: {
        node('linux') {
            checkout scm
            try {
                sh 'make'
            }
            finally {
                junit '**/target/*.xml'
            }
        }
    },
    windows: {
        node('windows') {
            /* .. snip .. */
        }
    }
}
```

## Multi-environment Pipeline

```yaml
pipeline {
  agent none
  stages {
    stage('Fluffy Build') {
      parallel {
        stage('Build Java 8') {
          agent {
            node {
              label 'java8'
            }

          }
          steps {
            sh './jenkins/build.sh'
            stash(name: 'Java 8', includes: 'target/**')
          }
        }
        stage('Build Java 7') {
          agent {
            node {
              label 'java7'
            }

          }
          steps {
            sh './jenkins/build.sh'
            archiveArtifacts 'target/*.jar'
            stash(name: 'Java 7', includes: 'target/**')
          }
        }
      }
    }
    stage('Fluffy Test') {
      parallel {
        stage('Backend Java 8') {
          agent {
            node {
              label 'java8'
            }

          }
          steps {
            unstash 'Java 8'
            sh './jenkins/test-backend.sh'
            junit 'target/surefire-reports/**/TEST*.xml'
          }
        }
        stage('Frontend') {
          agent {
            node {
              label 'java8'
            }

          }
          steps {
            unstash 'Java 8'
            sh './jenkins/test-frontend.sh'
            junit 'target/test-results/**/TEST*.xml'
          }
        }
        stage('Performance Java 8') {
          agent {
            node {
              label 'java8'
            }

          }
          steps {
            unstash 'Java 8'
            sh './jenkins/test-performance.sh'
          }
        }
        stage('Static Java 8') {
          agent {
            node {
              label 'java8'
            }

          }
          steps {
            unstash 'Java 8'
            sh './jenkins/test-static.sh'
          }
        }
        stage('Backend Java 7') {
          agent {
            node {
              label 'java7'
            }

          }
          steps {
            unstash 'Java 7'
            sh './jenkins/test-backend.sh'
            junit 'target/surefire-reports/**/TEST*.xml'
          }
        }
        stage('Frontend Java 7') {
          agent {
            node {
              label 'java7'
            }

          }
          steps {
            unstash 'Java 7'
            sh './jenkins/test-frontend.sh'
            junit 'target/test-results/**/TEST*.xml'
          }
        }
        stage('Performance Java 7') {
          agent {
            node {
              label 'java7'
            }

          }
          steps {
            unstash 'Java 7'
            sh './jenkins/test-performance.sh'
          }
        }
        stage('Static Java 7') {
          agent {
            node {
              label 'java7'
            }

          }
          steps {
            unstash 'Java 7'
            sh './jenkins/test-static.sh'
          }
        }
      }
    }
    stage('Confirm Deploy') {
      steps {
        input(message: 'Okay to Deploy to Staging?', ok: 'Let\'s Do it!')
      }
    }
    stage('Fluffy Deploy') {
      agent {
        node {
          label 'java7'
        }

      }
      steps {
        unstash 'Java 7'
        sh './jenkins/deploy.sh staging'
      }
    }
  }
}
```

## Post Section in Jenkins Pipeline

```yaml
pipeline {
  stages {
    stage('Buzz Build') {
      parallel {
        stage('Build Java 7') {
          steps {
            sh """
              echo I am a $BUZZ_NAME!
              ./jenkins/build.sh
            """
          }
          post {
            always {
              archiveArtifacts(artifacts: 'target/*.jar', fingerprint: true)
            }
            success {
              stash(name: 'Buzz Java 7', includes: 'target/**')
            }
          }
        }
  ...
}
```

## Environment Directive

- Examples include BUILD_NUMBER, JENKINS_URL and EXECUTOR_NUMBER

```groovy
pipeline {
    agent any
    environment {
        CC = 'clang'
    }
    stages {
        stage('Example') {
            environment {
                AN_ACCESS_KEY = "SECRET" //credentials('my-predefined-secret-text')
            }
            steps {
                sh 'printenv'
```

## Notifications

```groovy
stages {
  stage ('Start') {
    steps {
      // send build started notifications
      slackSend (color: '#FFFF00', message: "STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
    }
  }
}
```

```groovy
// send to email
emailext (
  subject: "STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
  body: """<p>STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
    <p>Check console output at &QUOT;<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>""",
  recipientProviders: [[$class: 'DevelopersRecipientProvider']]
)
```

### Conditional Notificaton

```groovy
// on success
post {
  success {
    slackSend (color: '#00FF00', message: "SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")

    emailext (
      subject: "SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
      body: """<p>SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
        <p>Check console output at &QUOT;<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>""",
      recipientProviders: [[$class: 'DevelopersRecipientProvider']]
    )
  }
}

// on fail
failure {
 slackSend (color: '#FF0000', message: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")

 emailext (
   subject: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
   body: """<p>FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
     <p>Check console output at &QUOT;<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>""",
   recipientProviders: [[$class: 'DevelopersRecipientProvider']]
  )
}
```

## WHEN DIRECTIVE

**BUILT-IN CONDITIONS**

```groovy
// branch — Execute the stage when the branch being built matches the branch pattern given:
when {
  branch 'master'
}

//environment — Execute the stage when the specified environment variable is set to the given value:
when {
  environment name: 'DEPLOY_TO', value: 'production'
}

//expression — Execute the stage when the specified expression evaluates to true:
when {
  expression {
    return params.DEBUG_BUILD
  }
}
```

**BUILT-IN NESTED CONDITIONS**

```groovy
// allOf — Execute the stage when all nested conditions are true:
when {
  allOf {
    branch 'master'
    environment name: 'DEPLOY_TO', value: 'production' // AND
  }
}

//anyOf — Execute the stage when at least one of the nested conditions is true
when {
  anyOf {
    branch 'master'
    branch 'staging' // OR
  }
}

// not — Execute the stage when the nested condition is false.
when { not { branch 'master' } }
```

```groovy
...
stage('Confirm Deploy to Staging') {
    when {
      branch 'master'
    }
    steps {
      input(message: 'Deploy to Stage', ok: 'Yes, let\'s do it!')
    }
  }
  stage('Deploy to Staging') {
    agent {
      node {
        label 'java8'
      }
    }
    when {
      branch 'master'
    }
    steps {
      unstash 'Buzz Java 8'
      sh './jenkins/deploy.sh staging'
    }
  }
...
```

## GIT ENVIRONMENT VARIABLES

```groovy
stage('Generate Reports') {
  steps {
    sh './jenkins/generate-reports.sh'
    sh 'tar -czv target/reports.tar.gz target/reports'
    archiveArtifacts 'target/*.tar.gz'
    echo "Finished run for commit ${ env.GIT_COMMIT.substring(0,6) }"
  }
}
```

# Start Jenkins inside a container

Refer **[Docker Hub](https://hub.docker.com/r/jenkins/jenkins/)**
## Run `jenkins` as a docker container

```
docker run \
  --name jenkins \
  --detach \
  --volume jenkins-data:/var/jenkins_home \
  --publish 8080:8080 \
  --publish 50000:5000 \
  jenkins/jenkins:lts
```  

where,
- `--name` - name of the container
- `--detach` or `-d` - detached mode (in background)
- `--volume jenkins-data:/var/jenkins_home` - bind named volume (will create a directory in `/var/lib/docker/volumes/`)
- `--publish 8080:8080`  or `-p` - map or export host port `8080` to container port `8080`
- `jenkins/jenkins:lts` - docker image to be used.
## Get the Password

- First Admin password can be found at `/var/jenkins_home/secrets/initialAdminPassword`

```
$ sudo docker exec -it 198b7deb5f7d /bin/bash
jenkins@198b7deb5f7d:/$ cat /var/jenkins_home/secrets/initialAdminPassword
8d53672878b24941a1cdf3df3c8ec8cc
jenkins@198b7deb5f7d:/$ exit
exit
```

- or check `docker logs CONTAINER_ID` and get the password.   

## Login to Jenkins GUI for the first time 

- Open a web browser and goto `localhost:8080`
- enter the password collected from previous step.
- Install Suggested plugins
- Create the first time user
- confirm jenkins url

## Sample docker-compose.yml

```
version: '3'
services:
  jenkins:
    tty: true
    stdin_open: true
    container_name: jenkins
    image: jenkins/jenkins
    ports:
      - "8080:8080"
    volumes:
      - "$PWD/jenkins_home:/var/jenkins_home"
    networks:
      - net
networks:
 net:
```

# Create Multibranch Pipeline with Git


# Learn Jenkins

- **[Jenkins Pipeline - Fundamentals](https://standard.cbu.cloudbees.com/cloudbees-university-jenkins-pipeline-fundamentals)** - **FREE** Course from CloudBees University
# Reference

- [Deploying Jenkins X in Linode Kubernetes Engine](https://www.linode.com/docs/kubernetes/how-to-deploy-jenkins-x-in-linode-kubernetes-engine/)
- [Installing Jenkins X using JX Boot (The Non CJXD edition)](http://sharepointoscar.com/2020-01-10-Installing-Jenkins-X/)
- [A collection of tutorials with JX](https://github.com/jenkins-x/jx-tutorial)
- [salaboy/products-service](https://github.com/salaboy/products-service)
- [salaboy/online-store](https://github.com/salaboy/online-store)
- [salaboy/customers-service](https://github.com/salaboy/customers-service)
- [cd.foundation](https://cd.foundation/)


more...
- [Complete Jenkins Pipeline Tutorial | Jenkinsfile explained](https://www.youtube.com/watch?v=7KCS70sCoK0)