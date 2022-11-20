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
                 app = docker.build("test:${env.BUILD_ID}")
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
                    app.push("${env.BUILD_ID}")
                    app.push("latest")
                    }
                }
            }
        }

        stage('Clone the Argo Manifest') {
            steps {
                script{
                    sh 'git clone --branch develop git@github.com:Nimeh/argo-manifests.git'
                    }
                }
            }
        


        stage('Update the image in yaml manifest/values file') {
            steps {
                script{
                    sh "cd argo-manifests"
                    sh "cd argo-manifests && git checkout develop"
                    sh "cd argo-manifests && sed -i '' 's#image.*#image: nginx#g' deployment.yaml"
                    sh "cd argo-manifests && git add ."
                    sh "cd argo-manifests && git commit -m 'changs'"
                    sh "cd argo-manifests && git push"
                    }
                }
            }

        stage('Raise A PR to Argo Repo') {
            steps {
                script{
                    sh "gh pr crate --title 'The bug is fixed' --body 'Everything works again'"
                    }
                }
            }      
    }
}
