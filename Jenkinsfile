pipeline {
    agent { label 'Prem-Slave' }

    environment {
        DEPLOY_BASE = 'C:\\JenkinsAgent\\deploy'
    }

    stages {
        stage('Checkout') {
            steps {
                echo '📦 Checking out code...'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo '🔨 Building the project...'
                bat '''
                    echo Compiling code...
                    if not exist build mkdir build
                    echo Build complete > build\\output.txt
                '''
            }
        }

        stage('Test') {
            steps {
                echo '🧪 Running tests...'
                bat 'echo All tests passed!'
            }
        }

        stage('Deploy to Staging') {
            steps {
                echo '🚀 Deploying to STAGING...'
                bat """
                    if not exist %DEPLOY_BASE%\\staging mkdir %DEPLOY_BASE%\\staging
                    copy build\\output.txt %DEPLOY_BASE%\\staging\\output.txt /Y
                """
            }
        }

        stage('Approve for Production') {
            steps {
                input message: '✅ Approve deployment to Production?', ok: 'Deploy'
            }
        }

        stage('Deploy to Production') {
            steps {
                echo '🚀 Deploying to PRODUCTION...'
                bat """
                    if not exist %DEPLOY_BASE%\\production mkdir %DEPLOY_BASE%\\production
                    copy build\\output.txt %DEPLOY_BASE%\\production\\output.txt /Y
                """
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline completed successfully.'
        }
        failure {
            echo '❌ Something failed!'
        }
        always {
            echo '🔁 Pipeline execution finished.'
        }
    }
}
