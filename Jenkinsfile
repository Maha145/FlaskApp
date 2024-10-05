pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                script {
                    // Determine the correct path format based on the environment
                    def workspacePath = isUnix() ? "${env.WORKSPACE}" : "${env.WORKSPACE}".replaceAll("\\\\", "/").replaceAll("C:/", "/c/")

                    // On Windows, use the Windows path format
                    if (!isUnix()) {
                        workspacePath = "${env.WORKSPACE}"
                    }

                    // Build the Docker image with the correct path
                    bat "docker build -t 'my-flask-app' ${workspacePath}"
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
                    // Convert the path for the Docker container
                    def workspacePath = isUnix() ? "${env.WORKSPACE}" : "${env.WORKSPACE}".replaceAll("\\\\", "/").replaceAll("C:/", "/c/")

                    docker.image('my-flask-app').inside("-v ${workspacePath}:${workspacePath} -w ${workspacePath}") {
                        sh 'docker inspect my-flask-app'
                    }
                }
            }
        }
    }
}
