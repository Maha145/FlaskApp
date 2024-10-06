pipeline {
    agent any
    environment {
        DOCKER_IMAGE = 'my-flask-app'
    }
   

        stage('Remove Old Docker Image') {
            steps {
                bat '''
                echo "Checking if the image exists..."
                docker images -q %DOCKER_IMAGE% || exit 0
                if exist (docker images -q %DOCKER_IMAGE%) (
                    echo "Removing old Docker image..."
                    docker rmi -f %DOCKER_IMAGE%
                ) else (
                    echo "No old Docker image found."
                )
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                bat '''
                echo "Building new Docker image..."
                docker build -t %DOCKER_IMAGE% .
                docker images | findstr %DOCKER_IMAGE%
                '''
            }
        }

        stage('Deploy to Local Docker') {
            steps {
                bat '''
                echo "Deploying the Docker container..."
                docker run -d -p 5000:5000 --name flask-app %DOCKER_IMAGE%
                '''
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed.'
            bat '''
            echo "Listing all running containers..."
            docker ps -a | findstr "flask-app"
            '''
        }
        failure {
            echo 'Build or deployment failed!'
        }
        success {
            echo 'Build and deployment successful!'
        }
    }
}
