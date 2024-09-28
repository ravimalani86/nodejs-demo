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

                    // List the contents of /var/www/html to verify the myapp directory
                    sh 'ls -l /var/www/html'

                    // Navigate to the app directory and install production dependencies
                    sh """
                    cd /var/www/html/myapp  // Ensure this path is correct
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
