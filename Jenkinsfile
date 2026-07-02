pipeline{
    agent any
    stages{
        stage('Clone repo'){
            steps{
                git branch: 'main', url: 'https://github.com/Christopherdsouza02/2-Tier-DevOps_Project.git'
            }
        }
        stage('Build image'){
            steps{
                sh 'docker build -t cdsouza404/flaskapp:v8 .'
            }
        }
        stage('Push Docker Image'){
            steps{
                sh 'docker push cdsouza404/flaskapp:v8'
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
               sh 'kubectl apply -f k8s/two-tier-app-deployment.yml'
            }
        }
    }
}
