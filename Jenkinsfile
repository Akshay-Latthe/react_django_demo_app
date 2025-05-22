pipeline {
    agent any

    stages {
        stage('Git code chekout') {
            steps {
                git branch: 'main',  url: 'https://github.com/Akshay-Latthe/react_django_demo_app.git'
                echo 'Code cloning  is done.'
            }
        }
        stage('Buid') {
            steps {
                echo 'This is code Building'
                sh 'docker build -t notes-app:latest .'
            }
        }
        stage('Docker push') {
            steps {
                echo 'Pushing docker image to dockerhub'
                withCredentials([usernamePassword // Correctly indented
                          (credentialsId:"dockerHubCreds",
                          usernameVariable:"dockerHubUser",
                          passwordVariable:"dockerHubPass")]){
                sh 'echo $dockerHubPass | docker login -u $dockerHubUser --password-stdin'
                sh "docker image tag notes-app:latest ${env.dockerHubUser}/notes-app:latest"
                sh "docker push ${env.dockerHubUser}/notes-app:latest"               
                }
            }
        }
        stage('Deployment') {
            steps {
                sh "docker compose down && docker compose up -d --build"
            }
        }
    }
}
