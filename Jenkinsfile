pipeline {
    agent any

    environment {
        DOCKER_USER = 'dakyh'
        BACKEND_IMAGE = "${DOCKER_USER}/filrouge-backend"
        FRONTEND_IMAGE = "${DOCKER_USER}/filrouge-frontend"
        DB_IMAGE = "${DOCKER_USER}/filrouge-db"
    }

    stages {
        stage('Cloner le dépôt') {
            steps {
                sh 'git clone https://github.com/dakyh/FilRouge.git'
            }
        }

        stage('Build des images') {
            steps {
                sh 'docker build -t $BACKEND_IMAGE:latest FilRouge/Backend'
                sh 'docker build -t $FRONTEND_IMAGE:latest FilRouge/Frontend'
                sh 'docker build -t $DB_IMAGE:latest FilRouge/DB_filRouge'
            }
        }

        stage('Push des images') {
            steps {
                withDockerRegistry([credentialsId: 'khady', url: '']) {
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
}
