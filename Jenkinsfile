pipeline {
    agent { label "dev-server" }
    stages{
        stage("Clone Code"){
            steps{
                git url: "https://github.com/Ozorba/nodeJS-CICD.git", branch: "main"
            }
        }
        stage("Build and Test"){
            steps{
                sh "docker build . -t nodeJS-app"
            }
        }
        stage("Push to Docker Hub"){
            steps{
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker tag nodeJS-app ${env.dockerHubUser}/nodeJS-app:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/nodeJS-app:latest"
                }
            }
        }
        stage("Deploy"){
            steps{
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
