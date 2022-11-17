pipeline {
    agent any
    options {
        skipStagesAfterUnstable()
    }
    stages {
         stage('Clone repository') { 
            steps { 
                script{
                checkout scm
                }
            }
        }

        stage('Login To ECR') { 
            steps { 
                script{
                 sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 264473175039.dkr.ecr.us-east-1.amazonaws.com'
                }
            }
        }

        stage('Build') { 
            steps { 
                script{
                 app = docker.build("underwater")
                }
            }
        }
        stage('Test'){
            steps {
                 echo 'Empty'
            }
        }
        stage('Deploy') {
            steps {
                script{
                        docker.withRegistry('264473175039.dkr.ecr.us-east-1.amazonaws.com', 'ecr:us-east-1:AWS') {
                    app.push("${env.BUILD_NUMBER}")
                    app.push("latest")
                    }
                }
            }
        }
    }
}
