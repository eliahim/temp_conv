pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "eliahimps/temp-conv:latest"
    }

    stages {

        stage('Clone Code') {
            steps {
                git 'https://github.com/USERNAME/temp-converter-app.git'
            }
        }

        stage('Docker Image Build') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE:latest .'
            }
        }

      
        stage('Docker Push') {
            steps {
                withCredentials([usernamePassword(
                credentialsId: 'dockerhub-cred',
                usernameVariable: 'USER',
                passwordVariable: 'PASS')]) {

                sh '''
                echo $PASS | docker login -u $USER --password-stdin
                docker push $DOCKER_IMAGE:latest
                '''
                }
            }
        }

     
        stage('Kubernetes Deployment') {
            steps {
                sh 'kubectl apply -f deployment.yaml'
            }
        }
    }
}
