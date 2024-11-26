pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                // Pull the latest code from the repository
                git branch: 'main', url: 'https://github.com/Jovan9876/CICD-Jenkins.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                // Set up Python environment and install dependencies
                sh 'python3 -m venv venv'
                sh './venv/bin/pip install --upgrade pip'
                sh './venv/bin/pip install pytest'
            }
        }
        stage('Run Tests') {
            steps {
                sh './venv/bin/pytest test_app.py'
            }
        }
        stage('Build Docker Image') {
            steps {
                // Build a Docker image for the application
                sh '''
                whoami
                groups
                ls -l /var/run/docker.sock
                docker --version
                '''
                sh 'ls'
                sh 'docker build -t my-app:latest .'
            }
        }
        stage('Deploy') {
            steps {
                script {
                    // Log in to Docker Hub using Jenkins credentials
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    }
                    // Tag and push the image
                    sh 'docker tag jovan9876/my-app:latest jovan9876/my-app:latest'
                    sh 'docker push jovan9876/my-app:latest'
                }
            }
        }
    }

    post {
        always {
            sh 'docker logout'
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
