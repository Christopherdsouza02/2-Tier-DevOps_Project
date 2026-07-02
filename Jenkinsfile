pipeline {
    agent any

    environment {
        IMAGE_NAME = "cdsouza404/flaskapp"
        IMAGE_TAG = "${BUILD_NUMBER}"
    }

    stages {
        
        stage('Build Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
            }
        }

        stage('Docker Login & Push Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh """
                    echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                    docker push ${IMAGE_NAME}:${IMAGE_TAG}
                    """
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh """
                kubectl set image deployment/two-tier-app \
                two-tier-app=${IMAGE_NAME}:${IMAGE_TAG}
                """
                sh "kubectl rollout status deployment/two-tier-app"
            }
        }
    }
}
