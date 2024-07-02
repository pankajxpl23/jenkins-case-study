pipeline {
    agent any

    environment {
        REPO_URL = 'https://github.com/pankajxpl23/jenkins-case-study.git'
        CONTAINER_NAME = 'website_container'
        IMAGE_NAME = 'website_image'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: "${REPO_URL}"
            }
        }

        stage('Build and Publish') {
            steps {
                script {
                    sh 'sudo docker build -t ${IMAGE_NAME} .'
                    if (env.BRANCH_NAME == 'master') {
                        sh 'sudo docker run -d -p 82:80 --name ${CONTAINER_NAME} ${IMAGE_NAME}'
                    }
                }
            }
        }

        stage('Clean Up') {
            steps {
                script {
                    sh 'sudo docker stop ${CONTAINER_NAME} || true'
                    sh 'sudo docker rm ${CONTAINER_NAME} || true'
                    sh 'sudo docker rmi ${IMAGE_NAME} || true'
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}

