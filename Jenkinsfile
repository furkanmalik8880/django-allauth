pipeline {
    agent any
    
    stages{
        stage("Code"){
            steps{
                echo "Pulling The Code"
                git url:"https://github.com/furkan6397/django-notes-app.git",branch: "main"
            }
        }
        stage("Build"){
            steps{
                echo "Building The Code"
                sh "docker build -t my-node-app ."
            }
        }
        stage("Push to Docker Hub"){
            steps{
                echo "Push The container"
                withCredentials([usernamePassword(credentialsId:"dokerhub",passwordVariable:"dokerhubPass",usernameVariable:"dokerhubUser")]){
                sh "docker tag my-node-app ${env.dokerhubUser}/my-node-app:latest"
                sh "docker login -u ${env.dokerhubUser} -p ${env.dokerhubPass}"
                sh "docker push ${env.dokerhubUser}/my-node-app:latest"
                }
            }
        }
        stage("Deploye"){
            steps{
                echo "deploye code on Container"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
