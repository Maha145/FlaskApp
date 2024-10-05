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

        stage('Cleanup') {
            steps {
                echo 'Cleaning up Docker images and containers...'
                bat 'docker rm -f my-flask-app-container'
                bat 'docker rmi my-flask-app'
                echo 'Cleanup completed.'
            }
        }
    }
}
