pipeline {
    agent any
    
    stages{
        stage("Clone Code"){
            steps{
                echo "Cloning the code"
                git url: "https://github.com/jaydesai18/django-notes-app.git", branch: "main"
            }
        }
        stage("build"){
            steps{
                echo "Building the code"
                sh "docker build -t my-note-app ."
            }
        }
        stage("Push to Docker Hub"){
            steps{
                echo "Pushing the image to docker hub"
                withCredentials([usernamePassword(credentialsId: "dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker tag my-note-app ${env.dockerHubUser}/my-note-app:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/my-note-app:latest"
                }
            }
        }
        stage("Deploy"){
            steps{
                echo "Deploying the container"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
