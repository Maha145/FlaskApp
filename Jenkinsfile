pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                script {
                    // Convert Windows paths to Unix paths if running on Windows
                    def workspacePath = isUnix() ? "${env.WORKSPACE}" : "${env.WORKSPACE}".replaceAll("\\\\", "/").replaceAll("C:/", "/c/")

                    // Build Docker image
                    docker.build("my-flask-app", workspacePath)
                }
            }
        }

        stage('Test') {
            steps {
                echo 'Testing application...'
                // Add your test commands here
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Run the Docker container with the fixed path
                    def workspacePath = isUnix() ? "${env.WORKSPACE}" : "${env.WORKSPACE}".replaceAll("\\\\", "/").replaceAll("C:/", "/c/")

                    docker.image('my-flask-app').inside("-v ${workspacePath}:${workspacePath} -w ${workspacePath}") {
                        sh 'docker inspect my-flask-app'
                    }
                }
            }
        }
    }
}
