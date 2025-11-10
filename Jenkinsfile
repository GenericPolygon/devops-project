pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/GenericPolygon/devops-project.git'
            }
        }

        stage('Install Backend Dependencies') {
            steps {
                dir('backend') {
                    bat 'npm install'
                }
            }
        }

        stage('Install Frontend Dependencies') {
            steps {
                dir('frontend') {
                    bat 'npm install'
                }
            }
        }

        stage('Build Frontend') {
            steps {
                dir('frontend') {
                    bat 'npm run build'
                }
            }
        }

        stage('Test Backend') {
            steps {
                dir('backend') {
                    // Run Jest tests; continue even if none exist
                    bat 'npm test --passWithNoTests'
                }
            }
        }

        stage('Deploy') {
            steps {
                echo '🚀 Starting backend server...'
                dir('backend') {
                    bat 'start /B node index.js'
                }
                echo '✅ Deployment simulated successfully (server running in background)'
            }
        }
    }

    post {
        success {
            echo '✅ Build and deployment succeeded!'
        }
        failure {
            echo '❌ Build failed!'
        }
    }
}
