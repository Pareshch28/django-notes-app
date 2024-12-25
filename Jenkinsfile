@Library('Pareshlibraries@main') _
pipeline {
    agent any

    stages {
        stage('code') {
            steps {
               script{
                   clone('https://github.com/Pareshch28/django-notes-app.git','main')
               }
            }
        }
        stage('Test Library') {
            steps {
                hello()
            }
        }
        stage('build') {
            steps {
                script{
                    docker_build('notes-app','Latest','Pareshch28')
                echo 'build stage successful'
            }
        }
        stage('build push') {
            steps {
                echo 'pushing image to docker hub'
                withCredentials([usernamePassword(credentialsId:'dockerhubcred',passwordVariable: 'dockerHubPass',usernameVariable:'dockerHubUser')]){
                sh 'docker login -u $dockerHubUser -p $dockerHubPass'   
                sh 'docker image tag notes-app:latest pareshch28/notes-app:latest'
                sh 'docker push pareshch28/notes-app:latest'
                }
            }
        }
        stage('deploy') {
            steps {
                echo 'deploying successful'
                sh 'docker compose up -d'
            }
        }
    }
}
