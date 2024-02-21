pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = 'docker-hub-credentials' // Credential ID for Docker Hub
        TOMCAT_IMAGE_NAME = 'your-dockerhub-username/custom-tomcat' // Your Docker Hub username and Tomcat image name
        TOMCAT_IMAGE_TAG = 'latest' // Tag for the Tomcat image
        ARTIFACT_NAME = 'your-artifact-name.war' // Name of the artifact to include in the Tomcat image
        ARTIFACT_PATH = 'path/to/your/artifact.war' // Path to the artifact to include in the Tomcat image
    }

    stages {
        stage('Compile') {
            steps {
                // Compile your application
                sh 'mvn compile'
            }
        }

        stage('Test') {
            steps {
                // Run your tests
                sh 'mvn test'
            }
        }

        stage('Package') {
            steps {
                // Package your application
                sh 'mvn package'
            }
        }

        stage('Build Custom Tomcat Image') {
            steps {
                // Build custom Tomcat image with the artifact
                script {
                    docker.build("${TOMCAT_IMAGE_NAME}:${TOMCAT_IMAGE_TAG}", "-f Dockerfile . --build-arg ARTIFACT_NAME=${ARTIFACT_NAME} --build-arg ARTIFACT_PATH=${ARTIFACT_PATH}")
                }
            }
        }

        stage('Push Custom Tomcat Image to Docker Hub') {
            steps {
                // Push the custom Tomcat image to Docker Hub
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_HUB_CREDENTIALS) {
                        docker.image("${TOMCAT_IMAGE_NAME}:${TOMCAT_IMAGE_TAG}").push()
                    }
                }
            }
        }
    }
}
