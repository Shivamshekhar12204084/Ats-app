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
                git branch: 'main', url: 'https://github.com/Shivamshekhar12204084/Ats-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat """
                docker build -t %DOCKERHUB_USER%/%IMAGE_NAME%:%IMAGE_TAG% .
                """
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-creds', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    bat """
                    echo %PASSWORD% | docker login -u %USERNAME% --password-stdin
                    docker push %DOCKERHUB_USER%/%IMAGE_NAME%:%IMAGE_TAG%
                    docker logout
                    """
                }
            }
        }
    }

    post {
        success {
            echo "✅ Image pushed successfully: ${DOCKERHUB_USER}/${IMAGE_NAME}:${IMAGE_TAG}"

            emailext (
                subject: "✅ Jenkins Job SUCCESS - Docker Image Pushed",
                body: """
                <p>Hello,</p>
                <p>The Jenkins job has completed <b>successfully</b>.</p>
                <p><b>Image:</b> ${DOCKERHUB_USER}/${IMAGE_NAME}:${IMAGE_TAG}</p>
                <p>Regards,<br>Jenkins</p>
                """,
                mimeType: 'text/html',
                to: 'shivamshekhar12204084@gmail.com'
            )
        }
        failure {
            echo "❌ Build or push failed!"
        }
    }
}
