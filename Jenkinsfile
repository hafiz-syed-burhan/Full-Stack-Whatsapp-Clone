pipeline {
    agent any

    environment {
        DOCKER_COMPOSE = '/usr/local/bin/docker-compose' // Path to Docker Compose (update if required)
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Checkout the code from GitHub repository
                checkout scm
            }
        }

        stage('Build Docker Images') {
            steps {
                script {
                    // Build the Docker images for both frontend and backend
                    echo 'Building Docker images...'
                    sh 'docker-compose -f docker-compose.yml build'
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    // Run tests for both frontend and backend
                    echo 'Running tests...'
                    sh 'docker-compose run --rm frontend npm test'
                    sh 'docker-compose run --rm backend npm test'
                }
            }
        }

        stage('Deploy Application') {
            steps {
                script {
                    // Deploy the application in detached mode
                    echo 'Deploying application...'
                    sh 'docker-compose up -d'
                }
            }
        }

        stage('Clean Up') {
            steps {
                script {
                    // Clean up any Docker images or containers that were created during the pipeline
                    echo 'Cleaning up Docker images and containers...'
                    sh 'docker-compose down'
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline succeeded! Deployment completed.'
        }
        failure {
            echo 'Pipeline failed! Please check the logs for errors.'
        }
    }
}
