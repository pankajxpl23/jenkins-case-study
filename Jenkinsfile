pipeline {
    agent any
    
    environment {
        REPO_URL = 'https://github.com/hshar/website.git'
        CONTAINER_NAME = 'website_container'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: "${REPO_URL}"
            }
        }

        stage('Build') {
            parallel {
                stage('Master Branch Build and Publish') {
                    when {
                        branch 'master'
                    }
                    steps {
                        script {
                            sh 'docker build -t website_image .'
                            sh 'docker run -d -p 82:80 --name ${CONTAINER_NAME} website_image'
                        }
                    }
                }

                stage('Develop Branch Build Only') {
                    when {
                        branch 'develop'
                    }
                    steps {
                        script {
                            sh 'docker build -t website_image .'
                        }
                    }
                }
            }
        }

        stage('Clean Up') {
            steps {
                script {
                    sh 'docker stop ${CONTAINER_NAME} || true'
                    sh 'docker rm ${CONTAINER_NAME} || true'
                    sh 'docker rmi website_image || true'
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

