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
                git branch: 'main',
                    url: 'https://github.com/dakyh/FilRouge.git'
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

    // Si tu veux ajouter les mails plus tard
    /*
    post {
        success {
            mail to: 'dykha2704@gmail.com',
                 subject: "✅ Déploiement réussi - Projet Fil Rouge",
                 body: "L'application a été déployée avec succès."
        }
        failure {
            mail to: 'dykha2704@gmail.com',
                 subject: "❌ Échec du déploiement - Projet Fil Rouge",
                 body: "Une erreur s'est produite lors du déploiement de l'application."
        }
        always {
            sh 'docker logout'
        }
    }
    */
}
