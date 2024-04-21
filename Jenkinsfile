pipeline {
    agent any
    
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
