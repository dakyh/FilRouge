pipeline {
    agent any
    environment {
        DOCKER_USER = 'dakyh'
        BACKEND_IMAGE = "${DOCKER_USER}/filrouge-backend"
        FRONTEND_IMAGE = "${DOCKER_USER}/filrouge-frontend"
        DB_IMAGE = "${DOCKER_USER}/filrouge-db"
    }
    stages {
        stage('Cloner le dépôt avec sous-modules') {
            steps {
                checkout([$class: 'GitSCM', 
                    branches: [[name: 'main']], 
                    extensions: [[$class: 'SubmoduleOption', 
                                  disableSubmodules: false, 
                                  parentCredentials: true, 
                                  recursiveSubmodules: true, 
                                  reference: '', 
                                  trackingSubmodules: false]], 
                    userRemoteConfigs: [[url: 'https://github.com/dakyh/FilRouge.git']]])
            }
        }
        stage('Vérifier structure du dépôt') {
            steps {
                bat 'dir Backend'
                bat 'dir Frontend'
                bat 'dir DB_filRouge'
            }
        }
        // Autres stages...
    }
}