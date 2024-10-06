pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out the code from the repository...'
                // Uncomment the following line to enable Git checkout
                // git 'https://github.com/Maha145/FlaskApp.git'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                // Simulate running tests
                echo 'No tests found. Skipping test stage.'
            }
        }

         stage('Cleanup') {
            steps {
                echo 'Cleaning up Docker images and containers...'
                bat 'docker rm -f my-flask-app-container || true' // Ignore errors if container doesn't exist
                bat 'docker rmi my-flask-app || true' // Ignore errors if image doesn't exist
                echo 'Cleanup completed.'
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo 'Deploying the application to Docker...'
                    
                    // Build the Docker image
                    bat 'docker build -t my-flask-app C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\FlaskAppPipeline'

                    // Run the Docker container
                    bat 'docker run -d --name my-flask-app-container -p 5000:5000 my-flask-app'
                    
                    echo 'Application deployed successfully!'
                }
            }
        }
    }
}
