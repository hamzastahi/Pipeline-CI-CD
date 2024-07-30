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
                    // Pull the database image from Docker Hub
                    sh "docker pull ${env.DOCKER_IMAGE_DB}"

                    // Remove existing database container if it exists
                    sh 'docker rm -f db-1 || true'
                    // Run the new database container
                    sh "docker run -d --name db-1 -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=mydb -p 3306:3306 ${env.DOCKER_IMAGE_DB}"

                    // Use Docker Compose to manage other parts of the deployment, if necessary
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
