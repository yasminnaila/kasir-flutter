pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = 'kasir-flutter'
        DOCKER_TAG = "${env.BUILD_NUMBER}"
        GITHUB_REPO = 'https://github.com/yasminnaila/kasir-flutter.git'
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
                    sh "docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} ."
                    sh "docker tag ${DOCKER_IMAGE}:${DOCKER_TAG} ${DOCKER_IMAGE}:latest"
                }
            }
        }
        
        stage('Test') {
            steps {
                script {
                    echo "Running tests..."
                    // Uncomment below if you have tests
                    // sh 'flutter test'
                }
            }
        }
        
        stage('Deploy') {
            steps {
                script {
                    // Stop and remove old container if exists
                    sh 'docker stop kasir-flutter-app || true'
                    sh 'docker rm kasir-flutter-app || true'
                    
                    // Run new container
                    sh "docker run -d --name kasir-flutter-app -p 8080:80 ${DOCKER_IMAGE}:latest"
                }
            }
        }
    }
    
    post {
        success {
            echo 'Pipeline berhasil! 🎉'
        }
        failure {
            echo 'Pipeline gagal! ❌'
        }
    }
}
