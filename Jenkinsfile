pipeline {
    agent any

    triggers {
        pollSCM('* * * * *')  // Adjust this trigger as needed
    }

    environment {
        DOCKERHUB_CREDENTIALS = credentials('1') 
        IMAGE_NAME_FRONTEND = 'bacem592/nouveaudossier-frontend:latest'  // Replace with your image name
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
                        writeFile file: 'Dockerfile', text: '''
                        FROM node:16
                        WORKDIR /app
                        COPY package*.json ./
                        RUN npm install
                        COPY . .
                        RUN npm run build
                        EXPOSE 3000
                        CMD ["npm", "start"]
                        '''
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
