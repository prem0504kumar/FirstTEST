pipeline {
    agent { label 'Prem-Slave' }

    environment {
        DEPLOY_DIR = "C:/JenkinsAgent/deploy"
    }

    stages {
        stage('Build') {
            steps {
                echo "🔨 Building the project..."
                bat '''
                    echo Compiling code...
                    mkdir build
                    echo Build complete > build/output.txt
                '''
            }
        }

        stage('Test') {
            steps {
                echo "🧪 Running tests..."
                bat 'echo All tests passed!'
            }
        }

        stage('Deploy to Staging') {
            when {
                branch 'main'
            }
            steps {
                echo "🚀 Deploying to STAGING..."
                bat '''
                    if not exist %DEPLOY_DIR%\\staging mkdir %DEPLOY_DIR%\\staging
                    copy build\\output.txt %DEPLOY_DIR%\\staging\\output.txt /Y
                '''
            }
        }

        stage('Approve for Production') {
            when {
                branch 'main'
            }
            steps {
                input message: '🟡 Approve deployment to PRODUCTION?'
            }
        }

        stage('Deploy to Production') {
            when {
                branch 'main'
            }
            steps {
                echo "🚀 Deploying to PRODUCTION..."
                bat '''
                    if not exist %DEPLOY_DIR%\\production mkdir %DEPLOY_DIR%\\production
                    copy build\\output.txt %DEPLOY_DIR%\\production\\output.txt /Y
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Deployment pipeline complete for ${env.BRANCH_NAME}"
        }
        failure {
            echo "❌ Something failed!"
        }
    }
}
