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
                    appImage = docker.build('sample-app:latest', '--build-arg SERVER_PORT=9000 .') + ":$BUILD_NUMBER"
                }
            }
        }
        stage('Publish') {
            steps {
                script {
                    docker.withRegistry('lnoowak/jenkins', 'dockerhub') {
                        dockerImage.push()
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
