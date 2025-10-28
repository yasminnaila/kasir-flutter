pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = 'kasir-flutter'
        DOCKER_TAG = "${env.BUILD_NUMBER}"
        GITHUB_REPO = 'https://github.com/yasminnaila/kasir-flutter.git'
        APP_PORT = '3000'  // Port untuk aplikasi
    }
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: "${GITHUB_REPO}"
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image
                    bat "docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} ."
                    bat "docker tag ${DOCKER_IMAGE}:${DOCKER_TAG} ${DOCKER_IMAGE}:latest"
                }
            }
        }
        
        stage('Test') {
            steps {
                script {
                    echo "Running tests..."
                    // Uncomment below if you have tests
                    // bat 'flutter test'
                }
            }
        }
        
        stage('Stop Old Container') {
            steps {
                script {
                    // Stop and remove old container if exists
                    bat 'docker stop kasir-flutter-app || exit 0'
                    bat 'docker rm kasir-flutter-app || exit 0'
                }
            }
        }
        
        stage('Deploy') {
            steps {
                script {
                    // Run new container
                    bat "docker run -d --name kasir-flutter-app -p ${APP_PORT}:80 ${DOCKER_IMAGE}:latest"
                    echo "Application deployed successfully!"
                    echo "Access at: http://localhost:${APP_PORT}"
                }
            }
        }
    }
    
    post {
        success {
            echo '✅ Pipeline berhasil!'
            echo "🚀 Aplikasi berjalan di http://localhost:${APP_PORT}"
        }
        failure {
            echo '❌ Pipeline gagal!'
        }
        always {
            echo "Build #${env.BUILD_NUMBER} selesai"
        }
    }
}
