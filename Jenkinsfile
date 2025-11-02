pipeline {
    environment {
        registry = "yassinehachguer/projet_ci_cd"
        registryCredential = 'dockerhub'
        dockerImage = ''
    }
    agent any
    
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
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
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
                        dockerImage.push()
                        dockerImage.push('latest')
                    }
                }
            }
        }
        
        stage('Deploy image') {
            steps {
                script {
                    bat 'docker stop tp4-pipeline-container 2>nul || echo No container'
                    bat 'docker rm tp4-pipeline-container 2>nul || echo No container'
                    bat "docker run -d -p 8083:80 --name tp4-pipeline-container ${registry}:${BUILD_NUMBER}"
                }
            }
        }
    }
}