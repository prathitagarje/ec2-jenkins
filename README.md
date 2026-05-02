# ec2-jenkins
jenkins | jenkins.pem | SG - Inbound - jenkins-8080 | t3.large, c7i-flex.large

cd Downloads 
chmod 400 jenkins.pem
ssh -i "jenkins.pem" ec2-user@ec2-52-204-224-228.compute-1.amazonaws.com

sudo yum update -y
sudo yum install wget tar tree python -y 

sudo yum install git -y 
git config --global user.name "Atul Kamble"
git config --global user.email "atul_kamble
git config --list 
 
sudo yum install docker -y 
sudo systemctl start docker 
sudo systemctl enable docker 
sudo docker login 
docker --version 

sudo yum install maven -y 
mvn -v

sudo yum install java-21-amazon-corretto.x86_64 -y
java --version 

// copy script from https://www.jenkins.io/doc/book/installing/linux/#red-hat-centos

sudo wget -O /etc/yum.repos.d/jenkins.repo \
    https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
sudo yum upgrade
# Add required dependencies for the jenkins package
sudo yum install fontconfig java-21-openjdk
sudo yum install jenkins
sudo systemctl daemon-reload

jenkins --version 
sudo systemctl start jenkins 
sudo systemctl enable jenkins
newgrp docker

docker ps
docker run hello-world

sudo usermod -aG docker $USER
sudo usermod -aG docker jenkins
sudo usermod -aG docker ec2-user
newgrp docker

docker ps

sudo systemctl restart docker
sudo systemctl restart jenkins


http://instance-public-ip:8080

sudo cat /var/lib/jenkins/secrets/initialAdminPassword

// paste password 

Settings >> plugins >> docker, docker pipeline, Blue Ocean, AWS Credentials Plugin

sudo systemctl restart jenkins 

pipeline {
    agent any

    stages {
        stage('dev') {
            steps {
                echo 'I am in dev'
                sh 'python --version'
            }
        }

        stage('staging') {
            steps {
                echo 'I am in stage'
                sh 'docker --version'
            }
        }
        stage('prod') {
            steps {
                echo 'I am in prod'
                sh 'mvn -v'
            }
        }
    }
}
-----------

🔹 Jenkins
🔹 What is Jenkins?
Open-source Automation Server
Used for CI/CD (Continuous Integration & Continuous Deployment)
Written in Java
Supports plugins-based architecture
Runs as standalone (WAR), service, Docker container
🔹 Core Concepts
🔹 CI (Continuous Integration)
Developers push code to Git

Jenkins automatically:

Pulls code
Builds
Tests
Generates reports
🔹 CD (Continuous Delivery/Deployment)
Automates:

Packaging
Deployment to Dev/Test/Prod
Infrastructure provisioning
🔹 Jenkins Architecture
Master (Controller)

Manages jobs
Schedules builds
Assigns work to agents
Stores configuration
Agent (Node/Slave)

Executes build jobs
Can be Linux/Windows/Mac
Can run inside Docker/Kubernetes
Communication via:

SSH
JNLP
WebSocket
🚀 Jenkins Agents – Theory & Key Points to Remember
1️⃣ What is a Jenkins Agent?
A Jenkins Agent (formerly called Slave) is a machine that connects to the Jenkins Controller and executes build jobs.

🔹 Controller = Brain (manages jobs, UI, scheduling) 🔹 Agent = Worker (executes builds, tests, deployments)

👉 Used for distributed builds, scalability, and workload separation.

2️⃣ Why Jenkins Agents are Needed?
✔️ Parallel builds ✔️ Load distribution ✔️ Different OS environments (Linux/Windows/macOS) ✔️ Tool-specific environments (Java, Docker, Node, etc.) ✔️ Isolate heavy workloads ✔️ Secure production deployments

3️⃣ Jenkins Architecture (Controller + Agents)
             +------------------+
             |  Jenkins         |
             |  Controller      |
             +------------------+
                     |
      ---------------------------------
      |               |               |
+-------------+  +-------------+  +-------------+
| Linux Agent |  | Windows     |  | Docker      |
| (Build)     |  | Agent       |  | Agent       |
+-------------+  +-------------+  +-------------+

4️⃣ Types of Jenkins Agents
🔹 1. Permanent Agent (Static Agent)
Always running
Manually configured
Suitable for stable infrastructure
🔹 2. Dynamic Agent
Created on demand
Auto-destroyed after job
Used in cloud/Kubernetes
🔹 3. Docker Agent
Runs job inside container
Clean environment every build
🔹 4. Kubernetes Agent
Uses Pod as agent
Very scalable
Common in DevOps pipelines
5️⃣ Agent Communication Methods
✔️ SSH (Linux) ✔️ Windows Service ✔️ JNLP (Java Web Start) ✔️ Kubernetes plugin ✔️ Docker plugin

6️⃣ Important Terminologies
Term	Meaning
Node	Machine configured in Jenkins
Agent	Worker node
Executor	Number of parallel jobs agent can run
Label	Tag used to target specific agent
Workspace	Directory where build runs


7️⃣ Jenkinsfile Example Using Agent
Declarative Pipeline

pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                sh 'echo Building...'
            }
        }
    }
}

Specific Agent Label

pipeline {
    agent { label 'linux' }
}

Docker Agent
pipeline {
    agent {
        docker {
            image 'node:18'
        }
    }
}

