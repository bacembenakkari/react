pipeline {
    agent any

    triggers {
        pollSCM('* * * * *')  // Adjust this trigger as needed
    }

    environment {
        DOCKERHUB_CREDENTIALS = credentials('1') 
        IMAGE_NAME_FRONTEND = 'bacem592/frontend-1'  // Replace with your image name
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/bacembenakkari/react'  // Replace with your repo URL
            }
        }

        stage('Build ReactJS Image') {
            steps {
                dir('test-app') {
                    script {
                        dockerImageFrontend = docker.build("${IMAGE_NAME_FRONTEND}")
                    }
                }
            }
        }

        stage('Test Docker Hub Login') {
            steps {
                sh '''
                echo "${DOCKERHUB_CREDENTIALS_PSW}" | docker login -u "${DOCKERHUB_CREDENTIALS_USR}" --password-stdin
                '''
            }
        }

        stage('Push Image to Docker Hub') {
            steps {
                script {
                    sh """
                    echo "${DOCKERHUB_CREDENTIALS_PSW}" | docker login -u "${DOCKERHUB_CREDENTIALS_USR}" --password-stdin
                    docker push ${IMAGE_NAME_FRONTEND}
                    """
                }
            }
        }
    }
}
