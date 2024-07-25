pipeline {
    agent any

    environment {
        // Utilisation des variables Docker du premier script
        DOCKER_IMAGE_BACKEND = 'fatimazahraerhmaritlemcani132/pfa-ci-cd-backend:v1.0'
        DOCKER_IMAGE_FRONTEND = 'fatimazahraerhmaritlemcani132/pfa-ci-cd-frontend:v1.0'
        DOCKER_IMAGE_DB = 'fatimazahraerhmaritlemcani132/mysql:v1.0'
    }

    stages {
        stage('Checkout') {
            steps {
                // Utilisation des données de votre dépôt GitHub
                git(
                    url: 'https://github.com/fatitlem/Pipeline-CI-CD.git',
                    branch: 'main',
                    credentialsId: 'PFA-PIPELINE'
                )
            }
        }

        stage('Setup Database') {
            steps {
                script {
                    // Lancer le service de la base de données
                    sh 'docker-compose up -d db'

                    // Attendre que la base de données soit prête
                    sh '''
                    while ! docker-compose exec db mysqladmin ping -h "127.0.0.1" --silent; do
                        echo "Attente de la connexion à la base de données..."
                        sleep 2
                    done
                    '''

                    // Optionnel : Exécuter des commandes SQL initiales
                    sh 'docker-compose exec db mysql -u root -pmysql -e "CREATE DATABASE IF NOT EXISTS my_database;"'
                }
            }
        }

        stage('Build Backend') {
            steps {
                script {
                    // Utiliser Maven pour construire le backend
                    docker.image('maven:3.9.8-eclipse-temurin-17').inside {
                        sh 'mvn clean package -DskipTests -f spring-boot-projeect/pom.xml'
                    }
                    // Construire l'image Docker du backend
                    docker.build("${env.DOCKER_IMAGE_BACKEND}", 'spring-boot-projeect')
                }
            }
        }

        stage('Test Backend') {
            steps {
                script {
                    // Exécuter les tests du backend
                    sh 'docker-compose run backend ./gradlew test'
                }
            }
        }

        stage('Build Frontend') {
            steps {
                script {
                    // Utiliser Node pour construire le frontend
                    docker.image('node:20.15.0').inside {
                        sh 'npm install --prefix frontend/sbr-stage'
                        sh 'npm run build --prefix frontend/sbr-stage'
                    }
                    // Construire l'image Docker du frontend
                    docker.build("${env.DOCKER_IMAGE_FRONTEND}", 'frontend/sbr-stage')
                }
            }
        }

        stage('Test Frontend') {
            steps {
                script {
                    // Exécuter les tests du frontend
                    sh 'docker-compose run frontend npm test'
                }
            }
        }

        stage('Docker Push') {
            steps {
                script {
                    // Se connecter à Docker Hub et pousser les images
                    sh "docker login -u fatimazahraerhmaritlemcani132 -p fati1234"
                    sh "docker push ${env.DOCKER_IMAGE_BACKEND}"
                    sh "docker push ${env.DOCKER_IMAGE_FRONTEND}"
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Utiliser Docker Compose pour le déploiement
                    sh 'docker-compose down'
                    sh 'docker-compose up -d db'

                    // Attendre que la base de données soit en bonne santé
                    sh 'until [ "$(docker inspect -f "{{.State.Health.Status}}" $(docker-compose ps -q db))" == "healthy" ]; do sleep 1; done'

                    sh 'docker-compose up -d backend'

                    // Attendre que le backend soit en bonne santé (si vous avez une vérification de santé pour le backend)
                    sh 'until [ "$(docker inspect -f "{{.State.Health.Status}}" $(docker-compose ps -q backend))" == "healthy" ]; do sleep 1; done'

                    sh 'docker-compose up -d frontend'
                }
            }
        }
    }

    post {
        always {
            // Nettoyer les ressources et l'espace de travail
            cleanWs()
        }
    }
}
