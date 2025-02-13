pipeline {
    agent any
    
    stages {
        stage ("Code") {
            steps {
                echo "Cloning the code"
                git url:"https://github.com/LondheShubham153/django-notes-app.git", branch:"main"
            }
        }
        stage ("Build") {
            steps {
                echo "Building the code"
                sh "docker build -t note-app ."
            }
        }
        stage ("Push") {
            steps {
                echo "Pushing the code to DockerHub"
                withCredentials([usernamePassword(credentialsId:"dockerHub",usernameVariable:"dockerHubUser",passwordVariable:"dockerHubPass")]){
                    sh "docker tag note-app ${env.dockerHubUser}/note-app:latest"
                    sh "docker login -u ${env.dockerHubUser} -p ${dockerHubPass}"
                    sh "docker push ${env.dockerHubUser}/note-app:latest"
                }
            }
        }    
        stage ("Deploy") {
            steps {
                echo "Deployment of the code"
                sh "docker run -d -p 8000:8000 rajaryan08/note-app:latest"
                sh "docker-compose up && docker-compose down -d"            
            }
        }
    }
}
