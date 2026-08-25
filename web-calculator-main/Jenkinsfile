pipeline {
    agent any

    tools {
        maven 'maven'
    }

    environment {
        AWS_REGION = "ap-south-1"
        AWS_ACCOUNT_ID = "516311263965"
        ECR_REPO = "web-calculator"

        IMAGE = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPO}"
    }

    stages {

        stage('SCM') {
            steps {
                git branch: 'main',
                url: 'https://github.com/ddevsec-blip/web-calculator.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Docker Image Build') {
            steps {
                sh '''
                docker build -t $IMAGE:${BUILD_NUMBER} .
                '''
            }
        }

        stage('Push Image to ECR') {
            steps {
                sh '''
                aws ecr get-login-password --region $AWS_REGION | \
                docker login --username AWS --password-stdin \
                $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com

                docker push $IMAGE:${BUILD_NUMBER}
                '''
            }
        }

    }
}
