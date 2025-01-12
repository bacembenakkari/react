pipeline {
    agent any
    
    triggers {
        pollSCM('* * * * *')
    }
    
    environment {
        DOCKERHUB_CREDENTIALS = credentials('1')
        IMAGE_NAME_FRONTEND = 'bacem592/nouveaudossier-frontend:latest'
    }
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/bacembenakkari/react'
            }
        }
        
        stage('Build ReactJS Image') {
            steps {
                dir('test-app') {  // Make sure this directory exists and contains your React app
                    script {
                        // First, verify package.json exists
                        
                        dockerImageFrontend = docker.build("${IMAGE_NAME_FRONTEND}", "-f Dockerfile .")
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