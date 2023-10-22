pipeline {
    agent any
    environment {
        ECR_REGISTRY = '070503547773.dkr.ecr.eu-central-1.amazonaws.com'
        ECR_REPOSITORY = 'ci-cd-demo'
        DOCKER_IMAGE = "${ECR_REGISTRY}/${ECR_REPOSITORY}"
    }
    stages {
        stage('Docker Build') {
            steps {
                script {
                    docker.build("$DOCKER_IMAGE:latest")
                }
            }
        }
        stage('Docker Push to ECR') {
            steps {
                withCredentials([[
                    credentialsId: 'awsCredentials', 
                    accessKeyVariable: 'AWS_ACCESS_KEY_ID', 
                    secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
                ]]) {
                    script {
                        // Login to ECR
                        sh "eval \$(aws ecr get-login --region eu-central-1 --no-include-email)"
                        
                        // Push to ECR
                        docker.image("$DOCKER_IMAGE:latest").push()
                    }
                }
            }
        }
    }
}