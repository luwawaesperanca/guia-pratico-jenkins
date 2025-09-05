pipeline {
    agent any
    stages {
        stage('Build Docker Image') {
            steps {
                script{
                    dockerapp = docker.build("lesperanca96/guia-pratico-jenkins:${env.BUILD_ID}", '-f ./src/Dockerfile ./src')
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
        stage('Deploy no kubernetes') {
            environment{
                tag_version = "${env.BUILD_ID}"
            }
           steps {
                sh 'minikube status -p minikube-linux-amd64 || minikube start -p minikube-linux-amd64'
                withKubeConfig([credentialsId:'kubeconfig']) {
                    sh "sed -i \"s/{{tag}}/${tag_version}/g\" ./k8s/deployment.yaml"
                    sh 'kubectl apply -f k8s/deployment.yaml'
                }
            }
        }
    }
}
