pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'waseemhajwane/docker-demo:latest'  // Change to your Docker Hub username
        DOCKER_CREDENTIALS = 'dockerhub-credentials'       // Set Jenkins Docker credentials
    }

    stages {
        stage('Checkout') {
            steps {
                echo '✅ Checkout stage'
                git branch: 'master', url: 'https://github.com/WaseemHajwane/DockerDemo.git'
            }
        }

        stage('Restore') {
            steps {
                echo '✅ Restore dependencies'
                bat 'dotnet restore Project/DockerDemo/DockerDemo.sln'
            }
        }

        stage('Build') {
            steps {
                echo '✅ Build application'
                bat 'dotnet build Project/DockerDemo/DockerDemo.sln --configuration Release'
            }
        }

        stage('Publish') {
            steps {
                echo '✅ Publish application'
                bat 'dotnet publish Project/DockerDemo/DockerDemo.sln --configuration Release --output ./publish'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo '🐳 Building Docker Image'
                bat 'docker build -t ${DOCKER_IMAGE} .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                echo '🚀 Pushing Docker Image to Docker Hub'
                withDockerRegistry(credentialsId: "${DOCKER_CREDENTIALS}") {
                    bat 'docker push ${DOCKER_IMAGE}'
                }
            }
        }

        stage('Deploy Docker Container') {
            steps {
                echo '🚀 Deploying Docker Container'
                bat 'docker stop docker-demo || true'
                bat 'docker rm docker-demo || true'
                bat 'docker run -d -p 8081:8080 --name docker-demo ${DOCKER_IMAGE}'
            }
        }
    }
}
