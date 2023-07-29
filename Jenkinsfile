pipeline {
    agent any 
    
    stages{
        stage("Code"){
            steps{
                echo "cloning the code"
                git url: "https://github.com/Surajnakate/django-notes-app.git",branch: "main"
            }
            
        }
        stage("Build"){
            steps{
                echo "building the image"
                sh "docker build -t notes-app ."
            }
        }
        stage("Push to Docker Hub"){
            steps{
                echo "pushing the image to docker Hub"
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker tag notes-app ${env.dockerHubUser}/notes-app:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/notes-app:latest"
                }
            }
        }
        stage("Deploy"){
            steps{
                echo "deploying the container"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
