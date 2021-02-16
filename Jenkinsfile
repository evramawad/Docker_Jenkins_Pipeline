pipeline {
    environment {
    registry = "naistangz/docker_automation"
    registryCredential = "dockerhub"
    dockerImage = ''
    PATH = "$PATH:/usr/local/bin"
        NEXUS_VERSION = "nexus3"
        NEXUS_PROTOCOL = "http"
        NEXUS_URL = "192.168.1.200:8082"
        NEXUS_REPOSITORY = "docker"
        NEXUS_CREDENTIAL_ID = "c09bf387-73e4-48b3-982d-b74f75f97a1f"        
}

    agent any
    stages {
            stage('Cloning our Git') {
                steps {
                git 'https://github.com/vamsijakkula/hellowhale.git'
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
                        docker.withRegistry('http://192.168.1.200:8082', 'c09bf387-73e4-48b3-982d-b74f75f97a1f') {
                        dockerImage.push()
                        }
                    }
                }
            }
            stage('Deploy App') {
                 steps {
                    script {
                        kubernetesDeploy(configs: "hellowhale.yml", kubeconfigId: "mykubeconfig")
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
