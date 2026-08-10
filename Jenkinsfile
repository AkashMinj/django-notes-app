@Library('shared') _
pipeline {
    agent any

    stages {
        stage('Hello'){
            steps{
                script{
                    hello()
                }
            }
        }

        stage('Code') {
            steps {
               script{
                   gitclone("https://github.com/AkashMinj/django-notes-app.git","main")
               }
            }
        }

        stage('Build') {
            steps {
                script{
                    dockerBuild()
                }
                
            }
        }

        stage('Push') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: "dockerHubCred",
                        passwordVariable: "dockerHubPass",
                        usernameVariable: "dockerHubUser"
                    )
                ]) {
                    sh 'echo "$dockerHubPass" | docker login -u "$dockerHubUser" --password-stdin'
                    sh 'docker image tag django_app:latest "$dockerHubUser"/notes-app:latest'
                    sh 'docker push "$dockerHubUser"/notes-app:latest'
                }
            }
        }

        stage('Deploy') {
            steps {
               script{
                   dockerDeploy()
               }
            }
        }
    }
}
