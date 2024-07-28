pipeline {
    agent {
        node{
          label 'dev'   
        }
    }

    stages {
        stage('Code CLone') {
            steps {
                git url: 'https://github.com/SachinRB/django-notes-app.git', branch: 'main'
                echo 'Code'
            }
        }
        stage('Build & Test') {
            steps {
                sh 'whoami'
                sh 'docker build . -t notes-app-jenkins:latest'
                echo 'Build'
            }
        }
        stage('Push To DockerHub') {
            steps {
                 echo 'Push To DockerHub'
                 withCredentials([usernamePassword(credentialsId: 'dockerCreds', usernameVariable: 'dockerHubUser', passwordVariable: 'dockerHubPass')]) {
                        //docker image oldimage ${env.dockerHubUser}/newimage
                        echo '*******With Cred******'
                       // echo "${env.dockerHubUser}"
                         sh "docker image tag notes-app-jenkins:latest ${env.dockerHubUser}/notes-app-jenkins:latest"
                         sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                         sh "docker push ${env.dockerHubUser}/notes-app-jenkins:latest"
                      
                        }
             }
        }
        stage('Deploy') {
            steps {
                sh 'docker compose up -d'
               // sh 'docker run -d -p 8000:8000 --name notes-app notes-app:latest'
                echo 'Docker compose up'
                echo 'Deploy'
            }
        }
    }
}
