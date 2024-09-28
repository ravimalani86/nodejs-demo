pipeline {
    agent any

    tools {
        nodejs 'NodeJS_20.17.0'  // Use Node.js version 20.17.0
    }

    stages {
        stage('Install Dependencies') {
            steps {
                script {
                    // Install npm dependencies
                    sh 'npm install'
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    // Build the project if needed
                    sh 'npm run build'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Navigate to the app directory and install production dependencies
                    sh """
                    cd /var/www/html/myapp
                    npm install --production
                    pm2 restart myapp || pm2 start server.js --name "myapp"
                    """
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed.'
        }
    }
}
