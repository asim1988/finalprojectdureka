pipeline {
    agent {
        label 'docker_slave'
    }
    environment {
        BRANCH_NAME = 'main'
    }
    post {

             always {

               archiveArtifacts artifacts: 'target/*.war', followSymlinks: false

             }

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

        stage('docker build ') {

                   steps {

              echo "docker building image..."

              echo '$WORKSPACE'

              sh 'ls -la $WORKSPACE'

              sh 'cd $WORKSPACE'

                sh 'docker build --file Dockerfile --tag asimbilal2020/finalproject:$BUILD_NUMBER .'

              sh script: 'ansible-playbook -i localhost, buildimage.yml'  

           }      

        }

        stage('push docker image') {

                  steps {

              echo "pushing image to docker hub..."

                    withDockerRegistry(credentialsId: 'DOCKER_HUB_LOGIN', url: 'https://index.docker.io/v1/') {

              sh 'docker push docker.io/asimbilal2020/finalproject:$BUILD_NUMBER'

                  }     

               }

           }

        stage('docker deploy') {

          steps {

            echo "deploying to docker container..."

            sh 'docker stop abc-container || true'

            sh 'docker rm -f abc-container || true'

            sh 'docker run -d -P --name abc-container asimbilal2020/finalproject:$BUILD_NUMBER'

            sh 'docker ps -a'

          }
        }
    }
}
