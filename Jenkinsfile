pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Checkout the code from the Git repository
                    echo 'Checking out code from Git repository...'
                    git 'https://github.com/Maha145/FlaskApp.git'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo 'Building Docker image...'
                    bat 'docker build -t my-flask-app . || (echo "Docker build failed" && exit 1)'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo 'Deploying Docker container...'
                    bat 'docker rm -f my-flask-app-container || echo "No running container to remove"'
                    bat 'docker run -d --name my-flask-app-container -p 5000:5000 my-flask-app || (echo "Failed to run Docker container" && exit 1)'
                }
            }
        }
    }

    post {
        always {
            script {
                echo 'Cleaning up...'
                bat 'docker rmi my-flask-app || echo "No image to remove"'
            }
        }
    }
}
