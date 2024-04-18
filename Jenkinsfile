pipeline {
    agent any
    
    stages{
        stage("Clone code"){
            steps{
                echo "Cloning the code"
                git url:"https://github.com/satish2709/django-notes-app.git", branch: "main"
            }
        }
    
        stage("Build"){
            steps {
                echo "Building the image"
                sh "docker build -t my-node-app ."
            }
        }
        
        stage("Push to Docker Hub"){
            steps{
                echo "Pushing the image to docker Hub"
                withCredentials([usernamePassword(credentialsId: "dockerHub", passwordVariable: "dockerHubPass",usernameVariable: "dockerHubUser")]){
                sh "docker tag my-node-app ${env.dockerHubUser}/my-node-app:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${dockerHubPass}" 
                sh "docker push ${env.dockerHubUser}/my-node-app:latest"
                }
            }
            
        }
        
        stage("Deploy"){
            steps{
                echo "Deploying the Cantainer"
                sh "docker-compose down && docker-compose up -d"
            }
            
        }
    }
}
