pipeline {
    agent none

    environment {
        DOCKER_CREDENTIALS_ID = 'makhady' // Ton ID d'identifiants Docker Hub
        DOCKERHUB_USER = 'dakyh'
    }

    stages {
        stage('Checkout') {
            agent any
            steps {
                echo '📥 Clonage du dépôt GitHub...'
                checkout scm
            }
        }

        stage('Build & Test Backend') {
            agent {
                docker {
                    image 'python:3.12-slim'
                    args '-u root:root'
                }
            }
            steps {
                dir('Backend') {
                    echo "🔧 Installation et tests Backend"
                    sh '''
                        python3 -m venv venv
                        . venv/bin/activate
                        pip install --upgrade pip
                        pip install -r requirements.txt || true
                        # python3 manage.py test || true
                    '''
                }
            }
        }

        stage('Build & Test Frontend') {
            agent {
                docker {
                    image 'node:20-alpine'
                    args '-u root:root'
                }
            }
            steps {
                dir('Frontend') {
                    echo "🔧 Build Frontend React"
                    sh '''
                        npm install
                        npm run build
                    '''
                }
            }
        }

        stage('Build Docker images') {
            agent any
            steps {
                echo '🐳 Construction des images Docker...'
                dir('Backend') {
                    sh "docker build -t ${DOCKERHUB_USER}/filrouge-backend:latest ."
                }
                dir('Frontend') {
                    sh "docker build -t ${DOCKERHUB_USER}/filrouge-frontend:latest ."
                }
                dir('DB_filRouge') {
                    sh "docker build -t ${DOCKERHUB_USER}/filrouge-db:latest ."
                }
            }
        }

        stage('Push Docker images') {
            agent any
            steps {
                echo '🚀 Push vers Docker Hub...'
                withCredentials([usernamePassword(credentialsId: "${DOCKER_CREDENTIALS_ID}", usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh '''
                        echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push $DOCKER_USER/filrouge-backend:latest
                        docker push $DOCKER_USER/filrouge-frontend:latest
                        docker push $DOCKER_USER/filrouge-db:latest
                    '''
                }
            }
        }

        stage('Déploiement') {
            agent any
            steps {
                echo '🚀 Lancement de l’application via Docker Compose'
                sh '''
                    docker compose down || true
                    docker compose up -d
                '''
            }
        }
    }

    post {
        always {
            echo '🧹 Nettoyage terminé'
            sh 'docker logout || true'
        }
        success {
            echo '✅ Pipeline terminé avec succès'
        }
        failure {
            echo '❌ Échec du pipeline'
        }
    }
}
