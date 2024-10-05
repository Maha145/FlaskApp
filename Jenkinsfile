pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Clone the Git repository
                checkout scm
            }
        }

        stage('Build') {
            steps {
                // Build the Docker image
                bat 'docker build -t my-flask-app C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\FlaskAppPipeline'
            }
        }

        // You may remove or comment out the Test stage if you don't have tests yet
        /*
        stage('Test') {
            steps {
                script {
                    bat 'docker run --rm my-flask-app python -m unittest discover -s tests'
                }
            }
        }
        */

        stage('Deploy') {
            steps {
                script {
                    // Placeholder for deployment steps
                    echo 'Deploy stage: Add deployment steps here.'
                }
            }
        }
    }

    post {
        always {
            // Stop and remove the running container if it exists (optional, ensure no container is using the image)
            bat 'docker ps -q --filter ancestor=my-flask-app | for /F "delims=" %i in (\'more\') do docker stop %i'

            // Forcefully clean up Docker image to prevent conflict errors
            bat 'docker rmi -f my-flask-app'
        }

        failure {
            echo 'Build or deployment failed!'
        }
    }
}
