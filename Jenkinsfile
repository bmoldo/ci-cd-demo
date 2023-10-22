pipeline {
    agent any
    environment {
        DOCKER_IMAGE = 'your-dockerhub-username/hello-world-app'
    }
    stages {
        stage('Docker Build') {
            steps {
                script {
                    docker.build("$DOCKER_IMAGE:latest")
                }
            }
        }
        stage('Docker Push') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub', variable: 'DOCKERHUB_TOKEN')]) {
                    script {
                        docker.withRegistry('https://index.docker.io/v1/', 'dockerhub') {
                            docker.image("$DOCKER_IMAGE:latest").push()
                        }
                    }
                }
            }
        }
    }
}
