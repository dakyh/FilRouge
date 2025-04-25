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
                bat 'docker build -t %BACKEND_IMAGE%:latest Backend'
                bat 'docker build -t %FRONTEND_IMAGE%:latest Frontend'
                bat 'docker build -t %DB_IMAGE%:latest DB_filRouge'
            }
        }
        
        stage('Push des images') {
            steps {
                withDockerRegistry([credentialsId: 'khady', url: '']) {
                    bat 'docker push %BACKEND_IMAGE%:latest'
                    bat 'docker push %FRONTEND_IMAGE%:latest'
                    bat 'docker push %DB_IMAGE%:latest'
                }
            }
        }
        
        stage('Déploiement') {
            steps {
                bat '''
                    docker-compose down || exit 0
                    docker-compose pull
                    docker-compose up -d
                '''
            }
        }
    }
    
    // Vous pouvez décommenter cette section quand vous aurez configuré le serveur de mail
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
            bat 'docker logout'
        }
    }
    */
}
