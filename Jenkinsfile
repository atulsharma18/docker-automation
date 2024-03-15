pipeline {
    agent any
    tools{
        maven 'maven'
    }
    environment {
        // Define the path to Docker CLI binary inside WSL2
        DOCKER_CLI = '/usr/bin/docker'
    }
    stages{
        stage('Build Maven'){
            steps{
               checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/atulsharma18/docker-automation']])
               bat 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                     bat "wsl ${DOCKER_CLI} build -t atulsharma18/spring-boot-docker.jar ."
                }
              
            }
        }
        stage('Push docker image'){
            steps{
                script{
                    withCredentials([string(credentialsId: 'dockerhub-pwd', variable: 'dockerhubpwd')]) {
                    bat "wsl ${DOCKER_CLI} login -u atulsharma18 -p ${dockerhubpwd}"
}
                    bat "wsl ${DOCKER_CLI} push atulsharma18/spring-boot-docker.jar"
                }
            }
        }
        
    }
}