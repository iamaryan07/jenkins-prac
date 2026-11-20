@Library("Shared") _

pipeline {
    agent {
        label 'wsl-agent'
    }

    stages {
        stage('Code') {
            steps {
                script {
                    clone("https://github.com/iamaryan07/jenkins-prac.git", "main")
                }
            }
        }

        stage('Build') {
            steps {
                sh 'docker build -t notes-app:latest .'
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerHubCred',
                    passwordVariable: 'dockerHubPass',
                    usernameVariable: 'dockerHubUser'
                )]) {
                    sh '''
                        printf '%s' "$dockerHubPass" | docker login \
                            -u "$dockerHubUser" --password-stdin

                        docker image tag notes-app:latest \
                            "$dockerHubUser/notes-app:latest"

                        docker push "$dockerHubUser/notes-app:latest"
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    docker compose pull django_app
                    docker compose up -d --no-build
                '''
            }
        }
    }
}
