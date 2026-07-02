pipeline {
    agent any

    stages {

        stage('Clone Repo') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Christopherdsouza02/2-Tier-DevOps_Project.git'
            }
        }

        stage('Build Image') {
            steps {
                sh 'docker build -t cdsouza404/flaskapp:v8 .'
            }
        }

        stage('Docker Login & Push Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                    echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                    docker push cdsouza404/flaskapp:v8
                    '''
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s/two-tier-app-deployment.yml'
            }
        }
    }
}
