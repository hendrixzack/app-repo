pipeline {
    agent any

    stages {
        stage('Restart Deployment') {
            steps {
                script {
                    withAWS(credentials: 'aws_creds', region:'us-east-1') {
                        sh 'aws eks update-kubeconfig --name Rocky-Cluster'
                }
                               
            }
        }
        }

        stage('Build Docker image') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'rocky', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'
                        sh 'docker build -t dj322/rockyhtml .'
                        sh 'docker push dj322/rockyhtml'
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl get nodes'
                sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl apply -f service.yaml'
                sh 'kubectl get service rocky-service'
                sh 'kubectl rollout restart deployment rocky-deployment'
            }
        }
    }
}
