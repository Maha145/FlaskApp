pipeline {
    agent any
    environment {
        DOCKER_IMAGE = 'my-flask-app'
    }
    stages {
        stage('Checkout Code') {
            steps {
                echo "Hello check "
              //  git branch: 'main', url: 'https://github.com/Maha145/FlaskApp.git'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                bat '''
                docker build -t %DOCKER_IMAGE% .
                '''
            }
        }
        
        stage('Remove Old Docker Image') {
            steps {
                bat '''
                docker images -q %DOCKER_IMAGE% > nul
                if errorlevel 1 (
                    echo "No existing image to remove."
                ) else (
                    docker rmi %DOCKER_IMAGE%
                )
                '''
            }
        }
        
        stage('Deploy to Local Docker') {
            steps {
                bat '''
                docker run -d -p 5000:5000 --name flask-app %DOCKER_IMAGE%
                '''
            }
        }
    }
    
    post {
        always {
            echo 'Pipeline completed.'
            bat '''
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
