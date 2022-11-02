pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git branch: 'jenkins_lesson2', url: 'https://github.com/lnoowak/jenkins_lesson2.git'
            }
        }
        stage('Build docker image') {
            steps {
                script {
                    appImage = docker.build('sample-app:latest', '--build-arg SERVER_PORT=9000 .')
                }
            }
        }
        stage('Publish') {
            steps {
                script {
                    docker.withRegistry('https://hub.docker.com/repository/docker/lnoowak/jenkins', 'dockerhub') {
                        appImage.push()
                    }
                }
            }
        }
    }
    post {
        cleanup {
            script {
                cleanWs()
                sh "docker image rm ${appImage.id}"
            }
        }
    }
}
