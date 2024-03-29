pipeline {
    agent {
        label 'docker_slave'
    }
    environment {
        DOCKER_REPO = 'finalproject' // Docker Hub username or organization name
        IMAGE_NAME = 'asimbilal1' // Name of your Docker image
        IMAGE_TAG = 'latest' // Tag for your Docker image
        DOCKER_CREDENTIALS_ID = 'docker_credential' // Jenkins credentials ID for Docker Hub
        }
    stages {
        stage('Compile') {
            steps {
                sh '/opt/maven/bin/mvn clean'
                sh '/opt/maven/bin/mvn compile'
            }
        }
        stage('Test') {
            steps {
                sh '/opt/maven/bin/mvn test'
            }
        }
        stage('Package') {
            steps {
                sh '/opt/maven/bin/mvn package'
            }
        }
       
        stage('Build Docker Image') {
            steps {
                script {
                // Build your Docker image here (replace the placeholder commands with your actual build commands)
                sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'
            }
        }
    }
    
        stage('Push Docker Image to Docker Hub') {
            steps {
                script {
                withCredentials([usernamePassword(credentialsId: DOCKER_CREDENTIALS_ID, usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh "docker login -u $DOCKER_USER -p $DOCKER_PASS"
                    sh "docker tag $IMAGE_NAME:$IMAGE_TAG $DOCKER_REPO/$IMAGE_NAME:$IMAGE_TAG"
                    sh "docker push $DOCKER_REPO/$IMAGE_NAME:$IMAGE_TAG"
                    sh "docker logout"
                 }
             }
         }
     }
  }
}
