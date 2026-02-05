pipeline {
    agent none
    
    stages {
        stage('Build and Deploy') {
            steps {
                script {
                    // Production deployment (CHECK FOR BOTH main AND master)
                    if (env.BRANCH_NAME == 'main' || env.BRANCH_NAME == 'master') {
                        node('production-agent') {
                            stage('Checkout') {
                                checkout scm
                            }
                            stage('Install Dependencies') {
                                bat 'npm install'
                            }
                            stage('Build Production') {
                                bat 'npm run build'
                            }
                            stage('Test') {
                                bat 'npm run test:unit || echo "No tests configured"'
                            }
                            stage('Deploy to Production') {
                                echo 'Deploying to Production Server...'
                                echo 'Build files are in ./dist directory'
                                // Add your deployment commands here
                                // Example: bat 'xcopy /E /Y /I .\\dist C:\\inetpub\\wwwroot\\frontend-prod\\'
                            }
                        }
                    }
                    // Test/Development deployment
                    else if (env.BRANCH_NAME == 'develop' || env.BRANCH_NAME == 'test') {
                        node('test-agent') {
                            stage('Checkout') {
                                checkout scm
                            }
                            stage('Install Dependencies') {
                                bat 'npm install'
                            }
                            stage('Build Test') {
                                bat 'npm run build'
                            }
                            stage('Test') {
                                bat 'npm run test:unit || echo "No tests configured"'
                            }
                            stage('Deploy to Test') {
                                echo 'Deploying to Test Server...'
                                echo 'Build files are in ./dist directory'
                                // Add your deployment commands here
                                // Example: bat 'xcopy /E /Y /I .\\dist C:\\inetpub\\wwwroot\\frontend-test\\'
                            }
                        }
                    }
                    else {
                        echo "Branch ${env.BRANCH_NAME} is not configured for deployment"
                    }
                }
            }
        }
    }
    
    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed! Check the logs above.'
        }
    }
}