pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'waseemhajwane/docker-demo:latest'
        DOCKER_CREDENTIALS = 'Dockerhub-credentials'
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
                bat 'dotnet restore DockerDemo/DockerDemo.csproj'
            }
        }

        stage('Build') {
            steps {
                echo '✅ Build application'
                bat 'dotnet build DockerDemo/DockerDemo.csproj --configuration Release'
            }
        }

        stage('Publish') {
            steps {
                echo '✅ Publish application'
                bat 'dotnet publish DockerDemo/DockerDemo.csproj --configuration Release --output ./publish'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo '🐳 Building Docker Image'
                bat """docker build -t ${env.DOCKER_IMAGE} -f DockerDemo/Dockerfile DockerDemo/"""
            }
        }

        stage('Push to Docker Hub') {
            steps {
                echo '🚀 Pushing Docker Image to Docker Hub'
                withDockerRegistry(credentialsId: "Dockerhub-credentials", url: "https://index.docker.io/v1/") {
                    bat """docker login -u waseemhajwane -p <your-access-token>"""
                    bat """docker push ${env.DOCKER_IMAGE}"""
                }
            }
        }

        stage('Deploy Docker Container') {
            steps {
                echo '🚀 Deploying Docker Container'
                bat 'docker stop docker-demo || exit 0'
                bat 'docker rm docker-demo || exit 0'
                bat "docker run -d -p 8080:8080 --name docker-demo ${env.DOCKER_IMAGE}"
            }
        }
    }
}
