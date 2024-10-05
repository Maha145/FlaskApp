pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the Git repository
                git 'https://github.com/Maha145/FlaskApp.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    bat 'docker build -t my-flask-app .'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Remove any existing containers with the same name
                    bat 'docker rm -f my-flask-app-container || echo "No running container to remove"'
                    
                    // Run the Docker container
                    bat 'docker run -d --name my-flask-app-container -p 5000:5000 my-flask-app'
                }
            }
        }
    }

    post {
        always {
            script {
                // Clean up Docker images after the pipeline is done
                bat 'docker rmi my-flask-app || echo "No image to remove"'
            }
        }
    }
}
