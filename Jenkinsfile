pipeline {
    agent {label "dev"};

    stages {
        stage("Code") {
            steps {
                git url: "https://github.com/IshuAgrawal11/two-tier-flaskapp.git", branch: "main"
            }
        }
        stage("Build") {
            steps {
                sh "docker build -t my-app ."
            }
        }
        stage("Push to Docker Hub") {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: "Docker-Hub-creds",
                    passwordVariable: "dockerHubPass",
                    usernameVariable: "dockerHubUser"
                )]) {
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker image tag my-app ${env.dockerHubUser}/my-app:latest"
                    sh "docker push ${env.dockerHubUser}/my-app:latest"
                }
            }
        }
        stage("Deploy") {
            steps {
                sh "docker compose up -d"
            }
        }
    } // End of stages

    post {
        success {
            emailext from: 'ishuagrawal1103@gmail.com',
                     to: 'ishuagrawal1103@gmail.com',
                     body: 'Build success for Demo CICD App',
                     subject: 'Build success for Demo CICD App'
        }
        failure {
            emailext from: 'ishuagrawal1103@gmail.com',
                     to: 'ishuagrawal1103@gmail.com',
                     body: 'Build Failed for Demo CICD App',
                     subject: 'Build Failed for Demo CICD App'
        }
    }
} // End of pipeline
