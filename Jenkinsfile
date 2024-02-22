pipeline {
    agent {
        label 'workernode1'
    }
    environment {
        BRANCH_NAME = 'main'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM',
                    branches: [[name: "${BRANCH_NAME}"]],
                    userRemoteConfigs: [[url: 'https://github.com/asim1988/finalprojectdureka']]])
            }
        }
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
                    sh "docker build --no-cache -t asimfinalproject1:latest ."
                }
            }
        }
        stage('Push Docker image to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: '3090fefd-68f8-450a-95c6-b45324548312', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh "docker login -u ${DOCKER_USER} -p ${DOCKER_PASS}"
                        sh 'docker tag asimfinalproject1:latest asimbilal2020/dockerci:asimfinalproject1dh'
                        sh 'docker push asimbilal2020/dockerci:asimfinalproject1dh'
                    
                        sh "docker logout"

                }
            }
        }
    }

