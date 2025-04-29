pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'flask-demo-image'
        REPO_URL = 'https://github.com/Marut51/demo-flask-docker.git' // Correctly quoted URL
    }

    stages {
        stage('Checkout Code') {
            steps {
                script {
                    // Clone the repository from GitHub
                    git branch: 'master', url: "${REPO_URL}"
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker images
                    echo 'Building Docker image for Flask app...'
                    sh 'docker-compose build'
                }
            }
        }

        stage('Run Containers') {
            steps {
                script {
                    // Run the containers in detached mode
                    echo 'Starting the containers...'
                    sh 'docker-compose up -d'
                }
            }
        }

        stage('Test Flask App') {
            steps {
                script {
                    // Optionally, test if Flask app is running (e.g., using curl)
                    echo 'Testing Flask app...'
                    sh 'curl -s http://localhost:5000 || exit 1'
                }
            }
        }

        stage('Test Jenkins') {
            steps {
                script {
                    // Optionally, test if Jenkins is up (e.g., using curl)
                    echo 'Testing Jenkins...'
                    sh 'curl -s http://localhost:8080 || exit 1'
                }
            }
        }

        stage('Cleanup') {
            steps {
                script {
                    // Clean up the containers after testing (optional)
                    echo 'Stopping and removing containers...'
                    sh 'docker-compose down'
                }
            }
        }
    }

    post {
        always {
            // Clean up if any errors occur
            echo 'Cleaning up resources...'
            sh 'docker-compose down'
        }
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the logs.'
        }
    }
}

