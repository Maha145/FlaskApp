pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                script {
                    docker.build('my-flask-app')
                }
            }
        }

        stage('Test') {
            steps {
                echo 'Testing application...'
                // You can add your test scripts here
            }
        }

        stage('Deploy') {
            steps {
                script {
                    docker.image('my-flask-app').inside {
                        sh 'docker run -d -p 5000:5000 my-flask-app'
                    }
                }
            }
        }
    }
}
