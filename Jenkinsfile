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
                // Added --network=host to fix DNS issues during npm install
                bat "docker build --network=host -t %IMAGE_NAME%:%IMAGE_TAG% ./frontend"
            }
        }

        stage('Run App with Docker Compose') {
            steps {
                echo "Running app using docker-compose..."
                bat "docker-compose down"
                bat "docker-compose up -d --build"
            }
        }
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
