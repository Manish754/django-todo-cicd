pipeline {
    agent any
    
    stages {
        stage("code") {
            steps {
                echo "cloning the code"
                git url: "https://github.com/Manish754/django-todo-cicd.git", branch: "develop"
            }
        }
        
        stage("build") {
            steps {
                echo "building the code"
                sh "docker build -t noteapp ."
            }
        }
        
        stage("push to dockerhub") {
            steps {
                echo "pushing the image to Docker Hub"
                withCredentials([usernamePassword(credentialsId: "dockerHub", passwordVariable: "dockerHubpass", usernameVariable: "dockerHubuser")]) {
                    sh "docker tag noteapp ${env.dockerHubuser}/noteapp:latest"
                    sh "docker login -u ${env.dockerHubuser} -p ${env.dockerHubpass}"
                    sh "docker push ${env.dockerHubuser}/noteapp:latest"
                    
                }
            }
        }
        
        stage("deploy") {
            steps {
                echo "deploying the container"
                sh "docker-compose down && docker-compose up -d "
                // Add deployment steps here if needed
            }
        }
    }
}
