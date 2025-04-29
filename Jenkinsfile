pipeline {
    agent any

    environment {
        IMAGE_NAME = "hms_application"
        IMAGE_TAG = "latest"
        CONTAINER_NAME = "hms_container"
        GIT_REPO_URL = "https://github.com/sahilku6/HMS.git"
    }

    stages {
        stage('Clone Repository') {
            steps {
                echo "Cloning source code from GitHub..."
                git branch: 'main', url: "${GIT_REPO_URL}"
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image..."
                bat "docker build -t %IMAGE_NAME%:%IMAGE_TAG% ./frontend"
            }
        }

        stage('Run App with Docker Compose') {
            steps {
                echo "Running app using docker-compose..."
                bat "docker-compose down"
                bat "docker-compose up -d --build"
            }
        }

        /*
        stage('Push Docker Image to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    bat 'echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin'
                    bat 'docker tag %IMAGE_NAME%:%IMAGE_TAG% %DOCKER_USER%/%IMAGE_NAME%:%IMAGE_TAG%'
                    bat 'docker push %DOCKER_USER%/%IMAGE_NAME%:%IMAGE_TAG%'
                }
            }
        }
        */
    }

    post {
        success {
            echo "Build and deployment completed successfully!"
        }
        failure {
            echo "Build failed. Please check logs."
        }
    }
}
