pipeline {
    agent any

    environment {
        // Define environment variables for Docker images
        DOCKER_IMAGE_BACKEND = 'fatimazahraerhmaritlemcani132/pfa-ci-cd-backend:v1.0'
        DOCKER_IMAGE_FRONTEND = 'fatimazahraerhmaritlemcani132/frontend-pipeline:v1.0'
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

        stage('Build Database') {
            steps {
                script {

                      sh "docker pull ${env.DOCKER_IMAGE_DB}"
                }
            }
        }

        stage('Build Backend') {
            steps {
                script {

                    sh "docker pull ${env.DOCKER_IMAGE_BACKEND}"
                }
            }
        }

        stage('Build Frontend') {
            steps {
                script {

                     sh "docker pull ${env.DOCKER_IMAGE_FRONTEND}"
                }
            }
        }

        stage('Deploy') {
            steps {
                script {

                               sh 'docker compose up -d'// Note: Ensure you have a docker-compose.yml file in your workspace
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
