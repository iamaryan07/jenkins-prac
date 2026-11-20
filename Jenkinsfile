@Library("Shared") _

pipeline {
    agent {
        label 'wsl-agent'
    }

    stages {
        stage('Code') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                script {
		    docker_build("notes-app", "latest", "aryanbalhara")
		}
            }
        }

        stage('Push to DockerHub') {
            steps {
                script {
		    push_dockerhub("notes-app", "latest", "aryanbalhara")
		}
            }
        }

        stage('Deploy') {
            steps {
                script {
		   deploy()
		}
            }
        }
    }
}
