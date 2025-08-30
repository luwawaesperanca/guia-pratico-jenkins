pipeline {
    agent any
    stages {
        stage('Build Docker Image') {
            steps {
                script{
                    dockerapp = docker.build("https://github.com/luwawaesperanca/guia-pratico-jenkins:${env.BUILD_ID}", '-f ./src/Dockerfile ./src')
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script{
                    dockerapp.withRegistery('https://hub.docker.com/repositories/lesperanca96', 'dockerhub'){
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
