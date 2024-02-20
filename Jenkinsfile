pipeline {
    agent any
    
    stages {
        stage('Change Branch') {
            steps {
                // Checkout the repository and switch to the 'main' branch
                script {
                    git branch: 'main', changelog: false, credentialsId: 'your-git-credentials', url: 'https://github.com/asim1988/finalprojectdureka.git'
                }
            }
        }
        
        stage('Build') {
            steps {
                // Compile and package the Maven project
                script {
                    sh '/opt/maven/bin/mvn clean package'
                }
            }
        }
        
        stage('Test') {
            steps {
                // Run tests (optional)
                script {
                    sh '/opt/maven/bin/mvn test'
                }
            }
        }
        
        stage('Archive') {
            steps {
                // Archive the generated artifact (e.g., JAR, WAR)
                archiveArtifacts 'target/*.war' // Adjust the pattern according to your artifact's extension
            }
        }
stage('Build Docker Image') {
            steps {
                script {
                    // Assuming your Dockerfile is located at the root of your project directory
                    //Define the directory path you want to change to
                 
                              sh 'docker build --no-cache -t edv1asim:V1 .'

                }
            }
        }
  environment {
        DOCKER_HUB_CREDENTIALS = '2cadfb61-08fe-40d9-b049-34a3e076223d' // Credential  Docker Hub
        DOCKER_IMAGE_NAME = 'asimbilal2020/edv1asim' // Your Docker Hub username and image name
        DOCKER_IMAGE_TAG = 'V1' // Tag for your image
    }
    
    stages {
        stage('Login to Docker Hub') {
            steps {
                script {
                    // Log in to Docker Hub
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_HUB_CREDENTIALS) {
                        // No need to do anything here; just logging in is sufficient
                    }
                }
            }
        }
        
        stage('Tag Docker Image') {
            steps {
                script {
                    // Tag the existing Docker image
                    docker.image("${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}").tag("${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}")
                }
            }
        }
        
        stage('Push Docker Image to Docker Hub') {
            steps {
                script {
                    // Push the Docker image to Docker Hub
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_HUB_CREDENTIALS) {
                        docker.image("${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}").push()
                    }
                }
            }
        }
    }
}
