pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/GenericPolygon/devops-project.git'
            }
        }

        stage('Backend Install') {
            steps {
                dir('backend') {
                    bat 'npm install'
                }
            }
        }

        stage('Frontend Install & Build') {
            steps {
                dir('frontend') {
                    bat 'npm install'
                    bat 'npm run build'
                }
            }
        }

        stage('Run Backend') {
            steps {
                dir('backend') {
                    bat 'node index.js'
                }
            }
        }
    }

    post {
        success {
            echo '✅ Build and deployment successful!'
        }
        failure {
            echo '❌ Build failed. Check console output.'
        }
    }
}
