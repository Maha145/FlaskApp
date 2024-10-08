pipeline {
    agent any
    environment {
        DOCKER_IMAGE = 'my-flask-app'
        CONTAINER_NAME = 'flask-app'
    }
    stages {
        

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

        stage('Stop and Remove Existing Container') {
            steps {
                bat '''
                echo "Checking if container %CONTAINER_NAME% is running..."
                docker ps -a -q --filter "name=%CONTAINER_NAME%" || exit 0
                if exist (docker ps -a -q --filter "name=%CONTAINER_NAME%") (
                    echo "Stopping and removing the existing container..."
                    docker stop %CONTAINER_NAME%
                    docker rm %CONTAINER_NAME%
                ) else (
                    echo "No existing container found."
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
                docker run -d -p 5000:5000 --name %CONTAINER_NAME% %DOCKER_IMAGE%
                '''
            }
        }
    }

   post {
        success {
            mail to: "mokafikry2001@gmail.com",
                 subject: "Build Success: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: "Good news! The build was successful."
        }

        failure {
            mail to: "mokafikry2001@gmail.com",
                 subject: "Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: "Oh no! The build failed. Please check the Jenkins console output."
        }
    }
}
