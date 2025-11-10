pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/GenericPolygon/devops-project.git', branch: 'main'
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
                    bat 'npm test --passWithNoTests'
                }
            }
        }

        stage('Deploy Full Stack App') {
            steps {
                echo '🚀 Starting full-stack app (backend + frontend)...'
                dir('backend') {
                    bat 'start /B node index.js'
                }
                echo '✅ Full-stack app is running at http://localhost:5000'
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
