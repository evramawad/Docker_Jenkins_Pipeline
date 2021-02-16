pipeline {
    environment {
    registry = "naistangz/docker_automation"
    registryCredential = "dockerhub"
    dockerImage = ''
    PATH = "$PATH:/usr/local/bin"
}

    agent any
    stages {
            stage('Cloning our Git') {
                steps {
                git 'https://github.com/evramawad/Docker_Jenkins_Pipeline.git'
                }
            }

            stage('Building Docker Image') {
                steps {
                    script {
                        dockerImage = docker.build registry + ":$BUILD_NUMBER"
                    }
                }
            }

            stage('Deploying Docker Image to Dockerhub') {
                steps {
                    script {
                        docker.withRegistry('192.168.1.200:8082', c09bf387-73e4-48b3-982d-b74f75f97a1f) {
                        dockerImage.push()
                        }
                    }
                }
            }

            stage('Cleaning Up') {
                steps{
                  sh "docker rmi $registry:$BUILD_NUMBER"
                }
            }
        }
    }
