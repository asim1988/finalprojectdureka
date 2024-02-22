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
        
        stage('compile') {
            steps {
                // clean and comoile
                script {
                    sh '/opt/maven/bin/mvn clean compile'
                }
            }
        }

          stage('test') {
            steps {
                // test the Maven project
                script {
                    sh '/opt/maven/bin/mvn test'
                }
            }
        }
        
        stage('package') {
            steps {
                // package the maven project
                script {
                    sh '/opt/maven/bin/mvn package'
                }
            }
        }
        
       
stage('Build Docker Image') {
            steps {
                script {
                    // Assuming your Dockerfile is located at the root of your project directory
                    //Define the directory path you want to change to
                 
                              sh 'docker build --no-cache -t btbullet:V1 .'

                }
            }
        }
    }
}
