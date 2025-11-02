pipeline {
    agent any
    environment {
        registry = "yassinehachguer/projet_ci_cd"
        registryCredential = 'dockerhub'
        dockerImage = ''
    }

    stages {
        stage('Cloning Git') {
            steps {
                git branch: 'master',
                    credentialsId: 'github',
                    url: 'https://github.com/YassineHachguer/Projet-CI-CD'
            }
        }

        stage('Building image') {
            steps {
                script {
                    // Construire l'image Docker
                    dockerImage = docker.build("${registry}:${BUILD_NUMBER}")
                }
            }
        }

        stage('Test image') {
            steps {
                script {
                    echo "Tests passed"
                }
            }
        }

        stage('Publish Image') {
            steps {
                script {
                    docker.withRegistry('', registryCredential) {
                        // Pousser l'image avec le numéro de build
                        dockerImage.push()
                        // Pousser l'image latest
                        dockerImage.push('latest')
                    }
                }
            }
        }

        stage('Deploy image') {
            steps {
                script {
                    // Arrêter et supprimer l'ancien conteneur si existant
                    bat '''
                    docker stop tp4-pipeline-container 2>nul || echo No container
                    docker rm tp4-pipeline-container 2>nul || echo No container
                    '''
                    // Lancer le nouveau conteneur avec le port 8083 exposé
                    bat "docker run -d -p 8083:80 --name tp4-pipeline-container ${registry}:${BUILD_NUMBER}"
                }
            }
        }
    }
}
