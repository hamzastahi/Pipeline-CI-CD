pipeline {
    agent any

    environment {
        DOCKER_IMAGE_BACKEND = 'fatimazahraerhmaritlemcani132/pfa-ci-cd-backend:v1.0'
        DOCKER_IMAGE_FRONTEND = ' fatimazahraerhmaritlemcani132/frontend-image:v1.0'
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

        stage('Pull Database Image') {
            steps {
                script {
                    sh "docker pull ${env.DOCKER_IMAGE_DB}"
                }
            }
        }

        stage('Pull Backend Image') {
            steps {
                script {
                    sh "docker pull ${env.DOCKER_IMAGE_BACKEND}"
                }
            }
        }

        stage('Build Frontend') {
            steps {
                script {
                    // Ensure the correct build context
                     sh "docker build -t ${env.DOCKER_IMAGE_FRONTEND} -f frontend/sbr-stage/Dockerfile frontend/sbr-stage"
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    sh 'docker rm -f frontend || true'
                    sh 'docker compose up -d'
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
