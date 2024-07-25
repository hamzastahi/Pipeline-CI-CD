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

        stage('Build ') {
            steps {
                script {
                    // Use Maven Docker image to build the backend

                        sh 'docker-compose up -d .'
                    }

                }
            }
        }

      

        stage('Deploy') {
            steps {
                script {
                    // Use Docker Compose to manage deployment
                    sh 'docker-compose down'
                    sh 'docker-compose up -d db'
                    // Wait for the database to be healthy
                    sh 'until [ "$(docker inspect -f "{{.State.Health.Status}}" $(docker-compose ps -q db))" == "healthy" ]; do sleep 1; done'
                    sh 'docker-compose up -d backend'
                    // Wait for the backend to be healthy (optional, if you have a healthcheck for backend)
                    sh 'until [ "$(docker inspect -f "{{.State.Health.Status}}" $(docker-compose ps -q backend))" == "healthy" ]; do sleep 1; done'
                    sh 'docker-compose up -d frontend'
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
