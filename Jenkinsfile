
pipeline {
    
    agent any
    
    environment{
        IMAGE_NAME = "aakanksha0017/myapp"
        IMAGE_TAG = "${BUILD_NUMBER}"
    }
    stages{
        stage('Clone Code'){
            steps {
                git branch: 'main', url: 'https://github.com/Aakanksha7777/ESG-Project-B.git'
            }
        }         
        stage('Build Docker Image'){
            steps {
                sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'
            }
         }
        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(
            credentialsId: 'Dockerhub-creds',
            usernameVariable: 'DOCKER_USER',
            passwordVariable: 'DOCKER_PASS'
        )]) {
            sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
            sh 'docker push $IMAGE_NAME:$IMAGE_TAG'
        }
    }
}
        stage('Update Kubernetes Deployment'){
            steps{

                sh """
                sed -i 's|image:.*|image:$IMAGE_NAME:$IMAGE_TAG|' deployment.yml
                """ 

                sh 'kubectl apply -f deployment.yml'
                sh 'kubectl apply -f service.yml'  
        
            }
        }
    }
}


