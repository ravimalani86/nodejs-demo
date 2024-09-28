pipeline {
    agent any

    tools {
        nodejs 'NodeJS_20.17.0'  // Use Node.js version 20.17.0
    }

    stages {
        stage('Deploy') {
            steps {
                script {
                    // Print the current working directory
                    sh 'pwd'

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
