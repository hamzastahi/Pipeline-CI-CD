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

        stage('Build Database') {
            steps {
                script {
                    // Remove existing database container if it exists
                    sh 'docker rm -f db-1 || true'
                    // Run the new database container
                    sh "docker run -d --name db-1 -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=mydb -p 3306:3306 ${env.DOCKER_IMAGE_DB}"
                }
            }
        }

        stage('Build Backend') {
            steps {
                script {
                    // Remove existing backend container if it exists
                    sh 'docker rm -f backend || true'
                    // Run the new backend container
                    sh "docker run -d --name backend -p 9192:9192 ${env.DOCKER_IMAGE_BACKEND}"
                }
            }
        }

        stage('Build Frontend') {
            steps {
                script {
                    // Remove existing frontend container if it exists
                    sh 'docker rm -f frontend || true'
                    // Run the new frontend container
                    sh "docker run -d --name frontend -p 3000:3000 ${env.DOCKER_IMAGE_FRONTEND}"
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Stop and remove existing containers
                               sh 'docker-compose down || true'
                               // Remove any existing containers that may conflict
                               sh 'docker rm -f db-1 backend frontend || true'
                               // Recreate and start containers using Docker Compose
                               sh 'docker-compose up -d'// Note: Ensure you have a docker-compose.yml file in your workspace
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
