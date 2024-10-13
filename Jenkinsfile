pipeline {
    agent any
     
    environment {
        DOCKER_IMAGE = 'my-flask-app'
        CONTAINER_NAME = 'flask-app'
    }

     

    stages {

 stage('Print User Info') {
            steps {
                script {
                    // This will only work if the user manually triggers the job in Jenkins
                    def userId = currentBuild.rawBuild.getCause(hudson.model.Cause$UserIdCause)?.getUserId()
                    def userName = currentBuild.rawBuild.getCause(hudson.model.Cause$UserIdCause)?.getUserName()
                    //def email =  currentBuild.rawBuild.getCause(hudson.model.Cause$UserIdCause)?.getUserEmail()
                    def userEmail = jenkins.model.Jenkins.instance.getUser(userId).getProperty(jenkins.tasks.MailAddressResolver$UserProperty)?.getAddress()

                        env.BUILD_USER = userName
                        env.BUILD_USER_EMAIL = userEmail

                        echo "Build triggered by: ${userName} (${userEmail})"


                    if (userId && userName) {
                        echo "Triggered by: ${userName} (ID: ${userId})"
                    } else {
                        echo "No user information available (probably triggered by a non-human action)."
                    }
                }
            }
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
              echo "Sending email to: ${env.BUILD_USER_NAME}"
            mail to: "mokafikry2001@gmail.com",
                 subject: "Build Success: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body:"The pipeline run for ${env.JOB_NAME} - Build #${env.BUILD_NUMBER} was successful. You can view the details at ${env.BUILD_URL}"
        }

        failure {
              echo "Sending email to: ${env.BUILD_USER_ID}"
              echo "Helllllllllllllllllllo"
            mail to: "mokafikry2001@gmail.com",
                 subject: "Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: "The pipeline run for ${env.JOB_NAME} - Build #${env.BUILD_NUMBER} has failed. You can view the details at ${env.BUILD_URL}"
        }
    }



}
