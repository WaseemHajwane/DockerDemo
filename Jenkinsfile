pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'waseemhajwane/docker-demo:latest'
        DOCKER_CREDENTIALS = 'dockerhub-credentials'
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
                bat 'dotnet restore DockerDemo.sln'
            }
        }

        stage('Build') {
            steps {
                echo '✅ Build application'
                bat 'dotnet build DockerDemo.sln --configuration Release'
            }
        }

        stage('Publish') {
            steps {
                echo '✅ Publish application'
                bat 'dotnet publish DockerDemo.sln --configuration Release --output ./publish'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo '🐳 Building Docker Image'
                bat "docker build -t ${env.DOCKER_IMAGE} -f DockerDemo/Dockerfile DockerDemo/"
            }
        }

        stage('Push to Docker Hub') {
            steps {
                echo '🚀 Pushing Docker Image to Docker Hub'
                withDockerRegistry(credentialsId: "${DOCKER_CREDENTIALS}", url: "https://index.docker.io/v1/") {
                    bat "docker push ${env.DOCKER_IMAGE}"
                }
            }
        }

        stage('Deploy Docker Container') {
            steps {
                echo '🚀 Deploying Docker Container'
                bat 'docker stop docker-demo || true'
                bat 'docker rm docker-demo || true'
                bat "docker run -d -p 8080:8080 --name docker-demo ${env.DOCKER_IMAGE}"
            }
        }
    }
}
