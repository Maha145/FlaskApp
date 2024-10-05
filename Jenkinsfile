pipeline {
    agent any  // Use any available agent

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out the code from the repository...'
               
            }
        }
        
        stage('Build') {
            steps {
                echo 'Building the application...'
                // Simulate a build process
                echo 'Build process completed successfully.'
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
                echo 'Deploying the application...'
                // Simulate a deployment process
                echo 'Deployment process completed successfully.'
            }
        }
    }
    
    post {
        always {
            echo 'This will always run after the pipeline completes.'
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
