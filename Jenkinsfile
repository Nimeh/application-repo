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

        stage('Build') { 
            steps { 
                script{
                 app = docker.build("test")
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
                    docker.withRegistry('https://264473175039.dkr.ecr.us-east-1.amazonaws.com', 'ecr:us-east-1:AWS') {
                    app.push("${env.BUILD_NUMBER}")
                    app.push("latest")
                    }
                }
            }
        }

        stage('Clone the Argo Manifest') {
            steps {
                script{
                    sh 'git clone git@github.com:Nimeh/argo-manifests.git'
                    }
                }
            }
        


        stage('Update the image in yaml manifest/values file') {
            steps {
                script{
                    sh "sed -i 's#image: *#image: nginx'"
                    }
                }
            }
        

    }
}
