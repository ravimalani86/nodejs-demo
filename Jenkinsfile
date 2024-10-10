pipeline {
    agent any

    tools {
        nodejs 'NodeJS_20.17.0'  // Use Node.js version 20.17.0
    }

    stages {
        stage('Deploy') {
            steps {
                script {
                    // Navigate to the app directory and install production dependencies
                    sh """
                    node -v && pm2 -v
                    cd /var/www/html/nodejs-demo
                    npm install --production
                    pm2 describe myapp || echo "myapp not found"
                    pm2 restart myapp || pm2 start "node server.js" --name "myapp"
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
