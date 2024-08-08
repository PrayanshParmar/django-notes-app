pipeline {
    
    agent { 
        node{
            label "dev"
            
        }
    }
    
     stages {
        stage('Clone Code') {
            steps{
                echo "Cloning the code"
                git url:"https://github.com/LondheShubham153/django-notes-app.git", branch: "main"
            }
        }
        stage('Build') {
            steps{
                echo "Building the code"
                sh "docker build -t my-note-app ."
            }        
        }
        stage('Push to Docker Hub') {
            steps{
                withCredentials([usernamePassword(credentialsId: "dockerHub", passwordVariable: "dockerHubPass", usernameVariable: "dockerHubUser")]) {
                    sh '''
                        echo "${dockerHubPass}" | docker login -u "${dockerHubUser}" --password-stdin
                        docker tag my-note-app ${dockerHubUser}/my-note-app:latest
                        docker push ${dockerHubUser}/my-note-app:latest
                    '''
                }
            }
        }
        stage('Deploy') {
            steps{
                echo "Deploying the container"
                sh "docker compose down && docker compose up -d"
            }      
        }
    }
}
