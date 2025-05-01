pipeline {
    agent any

    environment {
        DOCKER_USER = 'dakyh'
        BACKEND_IMAGE = "${DOCKER_USER}/filrouge-backend"
        FRONTEND_IMAGE = "${DOCKER_USER}/filrouge-frontend"
        DB_IMAGE = "${DOCKER_USER}/filrouge-db"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/main']],
                    doGenerateSubmoduleConfigurations: false,
                    extensions: [],
                    userRemoteConfigs: [[
                        url: 'https://github.com/dakyh/FilRouge.git'
                    ]]
                ])
            }
        }

        stage('Build des images') {
            steps {
                sh 'docker build -t $BACKEND_IMAGE:latest Backend'
                sh 'docker build -t $FRONTEND_IMAGE:latest Frontend'
                sh 'docker build -t $DB_IMAGE:latest DB_filRouge'
            }
        }

        stage('Push des images') {
            steps {
                withDockerRegistry([credentialsId: 'makhady', url: '']) {
                    sh 'docker push $BACKEND_IMAGE:latest'
                    sh 'docker push $FRONTEND_IMAGE:latest'
                    sh 'docker push $DB_IMAGE:latest'
                }
            }
        }

        stage('Déploiement') {
            steps {
                sh '''
                    docker-compose down || true
                    docker-compose pull
                    docker-compose up -d
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Pipeline exécuté avec succès"
        }
        failure {
            echo "❌ Échec de l'exécution du pipeline"
        }
    }
}
