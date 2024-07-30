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


                   sh "docker run -p 9192:99192 ${env.DOCKER_IMAGE_FRONTEND} --name backend"
                }
            }
        }

        stage('Build Frontend') {
            steps {
                script {

                    sh "docker run -p 3000:3000 ${env.DOCKER_IMAGE_FRONTEND} --name frontend/sbr-stage"
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Pull the database image from Docker Hub
                    sh "docker pull ${env.DOCKER_IMAGE_DB}"

                    // Use Docker Compose to manage deployment
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
