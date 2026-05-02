# ec2-jenkins
jenkins | jenkins.pem | SG - Inbound - jenkins-8080 | t3.large, c7i-flex.large


cd Downloads 

chmod 400 jenkins.pem

ssh -i "jenkins.pem" ec2-user@ec2-52-204-224-228.compute-1.amazonaws.com


sudo yum update -y

sudo yum install wget tar tree python -y 


sudo yum install git -y 

git config --global user.name "Prathita Garje"

git config --global user.email "prathita_garje"

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

