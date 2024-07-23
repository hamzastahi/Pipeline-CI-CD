pipeline {
    agent any

    environment {
        DOCKER_IMAGE_BACKEND = 'fatimazahraerhmaritlemcani132/pfa-ci-cd-backend:v1.0'
        DOCKER_IMAGE_FRONTEND = 'fatimazahraerhmaritlemcani132/pfa-ci-cd-frontend:v1.0'
        DOCKER_IMAGE_DB = 'fatimazahraerhmaritlemcani132/mysql:v1.0'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/fatitlem/Pipeline-CI-CD.git'
            }
        }

        stage('Build Backend') {
            steps {
                script {
                    docker.image('maven:3.9.8-jdk-17').inside {
                        sh 'mvn clean package -f spring-boot-projeect/pom.xml'
                    }
                    docker.build("${env.DOCKER_IMAGE_BACKEND}", 'spring-boot-projeect')
                }
            }
        }

        stage('Build Frontend') {
            steps {
                script {
                    docker.image('node:20').inside {
                        sh 'npm install --prefix frontend/sbr-stage'
                        sh 'npm run build --prefix frontend/sbr-stage'
                    }
                    docker.build("${env.DOCKER_IMAGE_FRONTEND}", 'frontend/sbr-stage')
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    sh 'docker-compose down'
                    sh 'docker-compose up -d'
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
