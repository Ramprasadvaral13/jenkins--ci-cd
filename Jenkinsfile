pipeline {
    agent {
        docker {
            image 'python:3.9' // Use a Docker image with Python installed
            args '-u root' // Run Docker container as root user (optional)
        }
    }

    environment {
        IMAGE_TAG = "${BUILD_NUMBER}"
    }

    stages {
        stage('Checkout') {
            steps {
                git credentialsId: 'credentials',
                url: 'https://github.com/Ramprasadvaral13/jenkins--ci-cd',
                branch: 'main'
            }
        }

        stage('Build Docker') {
            steps {
                script {
                    echo 'Build Docker Image'
                    sh "docker build -t ramprasadv7/flask-cal:${IMAGE_TAG} ."
                }
            }
        }

        stage('Static Code Analysis') {
            steps {
                script {
                    // Create and activate virtual environment
                    sh 'python -m venv venv'
                    sh 'source venv/bin/activate'
                    
                    // Install pylint
                    sh 'pip install pylint'
                    
                    // Run pylint on app.py
                    sh 'pylint app.py'
                    
                    // Deactivate virtual environment
                    sh 'deactivate'
                }
            }
        }

        stage('Push Docker Image') {
            environment {
                DOCKER_CREDENTIALS = credentials('docker-cred')
            }
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', url: "https://index.docker.io/v1/") {
                        docker.image("ramprasadv7/flask-cal:${IMAGE_TAG}").push()
                    }
                }
            }
        }
    }
}
