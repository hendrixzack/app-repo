pipeline {
    agent any

    stages {
        stage('Restart Deployment') {
            steps {
                script {
                    withAWS(credentials: 'aws_cred', region:'us-east-1') {
                        sh 'aws eks update-kubeconfig --name team4project3cluster'
                }
                               
            }
        }
        }

        stage('Build Docker image') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'docker', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'
                        sh 'docker build -t hendrixz/team4html .'
                        sh 'docker push hendrixz/team4html'
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl get nodes'
                sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl apply -f service.yaml'
                sh 'kubectl get service team4-service'
                sh 'kubectl rollout restart deployment team4-deployment'
            }
        }
    }
}
