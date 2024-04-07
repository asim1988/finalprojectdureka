pipeline {
    agent any
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
        stage('ansible-dockerbuild-push') {
            steps {
                echo "Building image and pushing to Docker Hub..."
                withDockerRegistry(credentialsId: 'DOCKER_HUB_LOGIN', url: 'https://index.docker.io/v1/') {
                    sh 'ansible-playbook -i localhost, buildimage.yml'
                }
            }
        }
        
        stage('Pull Docker Image') {
            steps {
                echo "Pulling Docker image from GitHub..."
                sh 'docker pull asimbilal2020/finalproject:26'
            }
        }
        
        stage('ansible-k8sdeploy-qa') {
            steps {
                sh 'ansible-playbook --inventory /etc/ansible/hosts playbook_k8deploy.yml'
            }
        }
    }
}
