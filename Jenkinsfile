pipeline {
    agent any

    tools {
        nodejs 'NodeJS_20.17.0'  // Using Node.js version 20.17.0
    }

    environment {
        VPS_USER = 'youruser'
        VPS_HOST = 'your-vps-ip'
        VPS_APP_DIR = '/var/www/myapp'
    }

    stages {
        stage('Install Dependencies') {
            steps {
                script {
                    // Install npm dependencies using the specified Node.js version
                    sh 'npm install'
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    // Run tests using the specified Node.js version
                    sh 'npm test'
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    // Build the project using the specified Node.js version
                    sh 'npm run build'
                }
            }
        }

        stage('Deploy to VPS') {
            steps {
                sshagent(credentials: ['vps-ssh-key']) {  // Use the SSH key added in Jenkins credentials
                    script {
                        // Archive the build files
                        sh "tar czf build.tar.gz *"

                        // Copy files to VPS using scp
                        sh """
                        scp -o StrictHostKeyChecking=no build.tar.gz ${VPS_USER}@${VPS_HOST}:${VPS_APP_DIR}
                        ssh -o StrictHostKeyChecking=no ${VPS_USER}@${VPS_HOST} 'cd ${VPS_APP_DIR} && tar xzf build.tar.gz && npm install --production'
                        """
                    }
                }
            }
        }

        stage('Restart Application on VPS') {
            steps {
                sshagent(credentials: ['vps-ssh-key']) {
                    script {
                        // Restart the Node.js application using PM2
                        sh """
                        ssh -o StrictHostKeyChecking=no ${VPS_USER}@${VPS_HOST} 'pm2 restart myapp || pm2 start server.js --name "myapp"'
                        """
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment to VPS successful!'
        }
        failure {
            echo 'Deployment to VPS failed.'
        }
    }
}
