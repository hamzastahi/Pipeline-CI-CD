pipeline {
    agent any

    environment {
        DOCKER_IMAGE_BACKEND = 'fatimazahraerhmaritlemcani132/pfa-ci-cd-backend:v1.0'
        DOCKER_IMAGE_FRONTEND = ' hamza1234566/frontend-fezz:1.0.0'
        DOCKER_IMAGE_DB = 'fatimazahraerhmaritlemcani132/mysql:v1.0'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the main branch from GitHub
                git branch: 'testhamza',
                    url: 'https://github.com/hamzastahi/Pipeline-CI-CD.git',
                    credentialsId: 'hamzastahi'
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

                     sh "docker pull ${env.DOCKER_IMAGE_FRONTEND} "
                }
            }
        }

        stage('Deploy') {
            steps {
                script {

                    sh 'docker compose up --build '
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
