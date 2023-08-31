pipeline{
    agent any

    environment{
        DOCKER_USERNAME = "robinthakur00"
        APP_NAME = "gitops-argocd-jenkins-app"
        IMAGE_TAG = "${BUILD_NUMBER}"
        IMAGE_NAME = "${DOCKERHUB_USERNAME}" + "/" + "${APP_NAME}"
        REGISTRY_CREDS = 'dockerhub'
    }
    stages{
        stage('Cleanup workspace'){
            steps{
                script{
                    cleanWs()
                }
            }
        }
        stage('Checkout SCM'){
            steps{
                script{
                    git credentialsId: 'github',
                    url: 'https://github.com/robinthakur00/gitops-argocd-jenkins.git', 
                    branch: 'master'
                }
            }
        }
        stage('Building Docker Image'){
            steps{
                script{
                    docker_image = docker.build "${IMAGE_NAME}"
                }
            }
        }
        stage('Push Docker Image'){
            steps{
                script{
                    docker.withRegistry('', REGISTRY_CREDS){
                        docker_image.push("$BUILD_NUMBER")
                        docker_image.push('latest')
                    }
                }
            }
        }
    }
}