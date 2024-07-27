pipeline {
    agent any

    environment {
        // Define environment variables for Docker images
        DOCKER_IMAGE_BACKEND = 'fatimazahraerhmaritlemcani132/pfa-ci-cd-backend:v1.0'
        DOCKER_IMAGE_FRONTEND = 'fatimazahraerhmaritlemcani132/pfa-ci-cd-frontend:v1.0'
        DOCKER_IMAGE_DB = 'fatimazahraerhmaritlemcani132/mysql:v1.0'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the main branch from GitHub
                git branch: 'main',
                    url: 'https://github.com/fatitlem/Pipeline-CI-CD.git',
                    credentialsId: 'PFA-PIPELINE'
            }
        }

        stage('Build Backend') {
            steps {
                script {
                    // Use Maven Docker image to build the backend
                    docker.image('maven:3.9.8-eclipse-temurin-17').inside {
                        sh 'mvn clean package -f spring-boot-projeect/pom.xml'
                    }
                    // Build Docker image for the backend
                    sh "docker build -t ${env.DOCKER_IMAGE_BACKEND} spring-boot-projeect"
                }
            }
        }

        stage('Build Frontend') {
            steps {
                script {
                    // Use Node Docker image to build the frontend
                    docker.image('node:20.15.0').inside {
                        sh 'npm install --prefix frontend/sbr-stage'
                        sh 'npm run build --prefix frontend/sbr-stage'
                    }
                    // Build Docker image for the frontend
                    sh "docker build -t ${env.DOCKER_IMAGE_FRONTEND} frontend/sbr-stage"
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Use Docker Compose to manage deployment
                    sh 'docker compose down'
                    sh 'docker compose up -d'
                }
            }
        }
    }

    post {
        always {
            // Clean up workspace after the build
            cleanWs()
        }
    }
}
