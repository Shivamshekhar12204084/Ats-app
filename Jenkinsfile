pipeline {
    agent any

    environment {
        IMAGE_NAME = 'my-ats-app'
        IMAGE_TAG = 'latest'
        DOCKERHUB_USER = 'shivamshekhar12204084' // Replace with your Docker Hub username
    }

    stages {
        stage('Clone Repo') {
            steps {
                git 'https://github.com/Shivamshekhar12204084/Ats-app.git' // Replace with your repo URL
            }
        }

        stage('Build Docker Image') {
            steps {
                sh """
                docker build -t ${DOCKERHUB_USER}/${IMAGE_NAME}:${IMAGE_TAG} .
                """
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh """
                    echo "$PASSWORD" | docker login -u "$USERNAME" --password-stdin
                    docker push ${DOCKERHUB_USER}/${IMAGE_NAME}:${IMAGE_TAG}
                    docker logout
                    """
                }
            }
        }
    }

    post {
        success {
            echo "✅ Image pushed successfully: ${DOCKERHUB_USER}/${IMAGE_NAME}:${IMAGE_TAG}"
        }
        failure {
            echo "❌ Build or push failed!"
        }
    }
}
