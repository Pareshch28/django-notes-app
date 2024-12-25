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
        stage('Code Quality Analysis') {
            steps {
                withSonarQubeEnv('LocalSonar') { // Refers to the configured SonarQube server
                    sh sonar-scanner \
                                 -Dsonar.projectKey=Sonarqube-testproject \
                                 -Dsonar.sources=. \
                                 -Dsonar.host.url=http://localhost:9000 \
                                 -Dsonar.token=sqp_fa91fa2c1a087e2bc2af3ad9f59d78f97fe494ce
                }
            }
        }
        stage('build') {
            steps {
                script{
                    docker_build('notes-app','latest','pareshch28')
                echo 'build stage successful'
            }
          }     
        }
        stage('build push') {
            steps {
                echo 'pushing image to docker hub'
                withCredentials([usernamePassword(credentialsId:'dockerhubcred',passwordVariable: 'dockerHubPass',usernameVariable:'dockerHubUser')]){
                sh 'docker login -u $dockerHubUser -p $dockerHubPass'   
                
                sh 'docker push pareshch28/notes-app:latest'
                }
            }
        }
        stage('deploy') {
            steps {
                echo 'deploying successful'
                sh 'docker-compose up -d'
                echo 'abbto chal gaya bhenchod'
            }
        }
    }
}
