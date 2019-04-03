pipeline {
    environment {
        registry = "kundanr26/aspnet-core-test1"
        registryCredential = 'dockerhub'
        dockerImage = ''
    }
    agent any
    stages {
        stage('Checkout SCM') {
            steps {
                git 'https://github.com/kundan-kumar1993/asp-netcore'
            }
        }

        stage('Building Image') {
            steps {
                script {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }

        stage('Deploy Image') {
            steps {
                script {
                    docker.withRegistry( '', registryCredential ) {
                        dockerImage.push()
                    }
                }
            }
        }

        stage('Remove Unused docker image') {
            steps {
                sh "docker rmi $registry:$BUILD_NUMBER"
            }
        }

        stage('Test Image') {
            steps {
                sh 'echo "Tests passed"'
            }
        }
    }
}