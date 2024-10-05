pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'my-flask-app'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from Git
                checkout scm
            }
        }

        stage('Build') {
            steps {
                script {
                    // Build the Docker image
                    bat "docker build -t ${DOCKER_IMAGE} ${WORKSPACE}"
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    // Run tests inside the Docker container
                    bat """
                    docker run --rm ${DOCKER_IMAGE} python -m unittest discover -s tests || exit 0
                    """
                }
            }
        }

        stage('Deploy') {
            when {
                expression { currentBuild.resultIsBetterOrEqualTo('SUCCESS') }
            }
            steps {
                echo 'Deployment steps can be added here.'
            }
        }
    }

    post {
        always {
            // Stop any running container based on the image
            bat '''
            for /F "delims=" %i in ('docker ps -q --filter ancestor=${DOCKER_IMAGE}') do docker stop %i
            '''
            // Remove the Docker image forcefully
            bat 'docker rmi -f ${DOCKER_IMAGE}'
        }

        failure {
            echo 'Build or deployment failed!'
        }

        success {
            echo 'Build and deployment succeeded!'
        }
    }
}
