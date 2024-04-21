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
		    sh '''
		    echo 'Build Docker Image'
		    docker build -t ramprasadv7/flask-cal:${IMAGE_TAG} .
		    '''
		}
	    }
	}

	stage('Static Code Analysis') {
    steps {
        // Create and activate virtual environment
        sh 'python -m venv venv'
        sh 'source venv/bin/activate'

        // Install pylint
        sh 'pip install pylint'

        // Run pylint
        sh 'pylint your_module.py'
    }
}

	stage('Push docker image') {
	    environment {
		DOCKER_CREDENTIALS = credentials('docker-cred')
	    }
	    steps {
		script {
		    withDockerRegistry([credentialsId: 'docker-cred', url: "https://index.docker.io/v1/"]) {
			docker.image("ramprasadv7/flask-cal:${IMAGE_TAG}").push()
		    }
		}
	    }
	}
     }
}
