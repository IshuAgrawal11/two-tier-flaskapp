pipeline {
    agent { label "dev" } 

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
                    sh "docker logout" // Security best practice
                }
            }
        }
        stage("Deploy") {
            steps {
                sh "docker compose up -d"
            }
        }
    } 

    post {
        success {
            emailext from: 'ishuagrawal1103@gmail.com',
                     to: 'ishuagrawal1103@gmail.com',
                     body: "Build success for Demo CICD App - Job #${env.BUILD_NUMBER}",
                     subject: 'Build success for Demo CICD App'
        } // Added missing brace
        failure {
            emailext from: 'ishuagrawal1103@gmail.com',
                     to: 'ishuagrawal1103@gmail.com',
                     body: "Build Failed for Demo CICD App - Check logs at ${env.BUILD_URL}",
                     subject: 'Build Failed for Demo CICD App'
        } // Added missing brace
    }
}
