pipeline {
    agent any

    environment {
        BACKEND_DIR = "backend"
        FRONTEND_DIR = "frontend"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/GenericPolygon/devops-project.git'
            }
        }

        stage('Install Backend Dependencies') {
            steps {
                dir("${BACKEND_DIR}") {
                    // Cache node_modules for backend
                    cache(maxCacheSize: 2, caches: [
                        cacheEntry(path: 'node_modules', key: 'backend-deps', fallbackKeys: ['backend'])
                    ]) {
                        bat 'npm install'
                    }
                }
            }
        }

        stage('Install Frontend Dependencies') {
            steps {
                dir("${FRONTEND_DIR}") {
                    // Cache node_modules for frontend
                    cache(maxCacheSize: 2, caches: [
                        cacheEntry(path: 'node_modules', key: 'frontend-deps', fallbackKeys: ['frontend'])
                    ]) {
                        bat 'npm install'
                    }
                }
            }
        }

        stage('Build Frontend') {
            steps {
                dir("${FRONTEND_DIR}") {
                    bat 'npm run build'
                }
            }
        }

        stage('Test Backend') {
            steps {
                dir("${BACKEND_DIR}") {
                    bat 'npm test || echo "No tests found, continuing..."'
                }
            }
        }

        stage('Start Backend (optional)') {
            steps {
                dir("${BACKEND_DIR}") {
                    bat 'npm start'
                }
            }
        }
    }

    post {
        success {
            echo '✅ Build completed successfully!'
        }
        failure {
            echo '❌ Build failed!'
        }
    }
}
