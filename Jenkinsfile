pipeline {
    agent any

    stages {
        stage('Code Clone from GitHub: Step-1') {
            steps {
                echo 'Cloning code from GitHub'
                git url: 'https://github.com/priyadarshi0811/node-todo-cicd.git', branch: 'Devops'
            }
        }
        stage('Code Build using Docker : Step-2') {
            steps {
                echo 'Building Docker images'
                sh 'docker build -t nodetodo:v1 .'
                echo 'Docker image build is done.'
            }
        }
        stage('Build and Push Docker Images: Step-3') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'DockerCred',
                    passwordVariable: 'dockerHubPass',
                    usernameVariable: 'dockerHubUser'
                )]) {
                    echo 'Logging in to Docker Hub'
                    sh 'docker login -u ${dockerHubUser} -p ${dockerHubPass}'
                    echo 'Tagging nodetodo Docker image'
                    sh 'docker tag nodetodo:v1 ${dockerHubUser}/simaple-node-app:node-todo'
                    echo 'Pushing Docker image to Docker Hub'
                    sh 'docker push ${dockerHubUser}/simaple-node-app:node-todo'  // Removed extra quote
                    echo 'Docker images pushed to DockerHub successfully!'
                }
            }
        }
        stage('Deploy to DockerFile') {
            steps {
                sh 'docker run -d -p 8000:8000 darshif5/simaple-node-app:node-todo'
            }
        }
    }
}
