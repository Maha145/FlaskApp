pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "my-flask-app"
    }

    stages {

        stage('Checkout') {
            steps {
                // Checkout code from the Git repository
                checkout scm
            }
        }

        stage('Build') {
            steps {
                script {
                    // Get the current workspace path
                    def workspacePath = "${env.WORKSPACE}"
                    // Build Docker image with the correct tag
                    bat "docker build -t ${DOCKER_IMAGE} ${workspacePath}"
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    // Run Docker container and execute tests inside
                    bat "docker run --rm ${DOCKER_IMAGE} python -m unittest discover -s tests"
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Example of Docker container deployment
                    bat "docker run -d -p 5000:5000 ${DOCKER_IMAGE}"
                }
            }
        }
    }

    post {
        always {
            // Clean up after the pipeline
            script {
                bat "docker rmi ${DOCKER_IMAGE}"
            }
        }

        success {
            // Notify of successful build (optional)
            echo "Build and deployment successful!"
        }

        failure {
            // Handle failures (optional)
            echo "Build or deployment failed!"
        }
    }
}
