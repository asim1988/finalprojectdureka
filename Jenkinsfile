pipeline {
    agent {
        label 'docker_slave'
    }
    environment {
        BRANCH_NAME = 'main'
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
                    sh "docker build --no-cache -t asimfinalproject1:latest ."
                }
            }
        }
    }
