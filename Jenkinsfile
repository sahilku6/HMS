pipeline {
    agent any

    environment {
        IMAGE_NAME = "hms_application"
        IMAGE_TAG = "latest"
        CONTAINER_NAME = "hms_container"
        GIT_REPO_URL = "https://github.com/sahilku6/HMS.git"
    }

    triggers {
        githubPush() // Trigger the pipeline on GitHub push events
    }

    stages {
        stage('Clone Repository') {
            steps {
                echo "Cloning source code from GitHub..."
                git branch: 'main', url: "${GIT_REPO_URL}"
            }
        }

        stage('Stop Previous Containers') {
            steps {
                echo "Stopping and removing existing containers (if any)..."
                bat 'docker-compose down || exit 0'
            }
        }

        stage('Build and Run with Docker Compose') {
            steps {
                echo "Building and starting app with Docker Compose..."
                bat 'docker-compose up -d --build'
            }
        }

        stage('Show Running Containers') {
            steps {
                echo "Listing running containers..."
                bat 'docker ps'
            }
        }


        stage('Cleanup Docker System (Optional)') {
            steps {
                echo "Cleaning up unused Docker resources..."
                bat 'docker system prune -f'
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
