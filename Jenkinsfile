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

        stage('Start Database') {
            steps {
                script {
                    // Pull and start the database container
                    sh "docker pull ${env.DOCKER_IMAGE_DB}"
                    sh "docker run -d --name db-1 -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=mydb -p 3306:3306 ${env.DOCKER_IMAGE_DB}"
                    sleep 30 // Wait for the database to initialize
                }
            }
        }

        stage('Build Backend') {
            steps {
                script {
                    // Build Docker image for the backend
                    sh "docker run -d -p 9192:9192 --name backend --link db-1 ${env.DOCKER_IMAGE_BACKEND}"
                }
            }
        }

        stage('Build Frontend') {
            steps {
                script {
                    // Build Docker image for the frontend
                    sh "docker run -d -p 3000:3000 --name frontend --link db-1 ${env.DOCKER_IMAGE_FRONTEND}"
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Use Docker Compose to manage deployment
                    sh 'docker-compose -f /var/jenkins_home/workspace/PFA-PIPELINE/docker-compose.yml up -d'
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
