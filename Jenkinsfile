pipeline {
    
    agent any 
    
    environment {
        // Define your image name and tag using environment variables
        IMAGE_NAME = 'huiarup/cicd-e2e'
        DOCKER_CREDENTIALS_ID = 'DockerID' // Ensure this matches the ID of your Docker credentials in Jenkins
    }
    
    stages {
        
        stage('Checkout') {
            steps {
                checkout scm: [$class: 'GitSCM', userRemoteConfigs: [[url: 'https://github.com/Aruphui/TODO.git']], branches: [[name: '*/main']]]
            }
        }

        stage('Build Docker') {
            steps {
                script {
                    sh "docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} ."
                }
            }
        }

        stage('Push the artifacts') {
            steps {
                script {
                    // Login to Docker registry before pushing
                    sh "echo 'Push to Repo'"
                    docker.withRegistry('', DOCKER_CREDENTIALS_ID) {
                        // Define the docker image variable based on the built image
                        def dockerImage = docker.image("${IMAGE_NAME}:${BUILD_NUMBER}")
                        dockerImage.push()
                    }
                }
            }
        }
    }
}
