pipeline {
    agent any 

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
                // Fixed comma and case sensitivity here
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
                // Removed --build to use the image we just pushed/tagged
                sh "docker compose up -d"
            }
        }
    }
}

post{
        success{
            script{
                emailext from: 'mentor@trainwithshubham.com',
                to: 'mentor@trainwithshubham.com',
                body: 'Build success for Demo CICD App',
                subject: 'Build success for Demo CICD App'
            }
        }
        failure{
            script{
                emailext from: 'mentor@trainwithshubham.com',
                to: 'mentor@trainwithshubham.com',
                body: 'Build Failed for Demo CICD App',
                subject: 'Build Failed for Demo CICD App'
            }
        }
    }
}
