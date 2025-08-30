pipeline {
    agent any
    stages {
        stage('Build Docker Image') {
            steps {
                script{
                    dockerapp = docker.build("luwawaesperanca/guia-pratico-jenkins:${env.BUILD_ID}", '-f ./src/Dockerfile ./src')
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script{
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub'){
                        dockerapp.push('latest')
                        dockerapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying...'
            }
        }
    }
}
