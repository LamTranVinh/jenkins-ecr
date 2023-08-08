pipeline {
    agent any
    
    environment {
        AWS_DEFAULT_REGION = 'ap-northeast-2'
        ECR_REPO_NAME = 'repo/jenkins'
        DOCKER_IMAGE_NAME = 'vinh'
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    def dockerImage = docker.build("${DOCKER_IMAGE_NAME}:${env.BUILD_NUMBER}")
                }
            }
        }
        
        stage('Push to ECR') {
            steps {
                script {
                    def awsAccountId = sh(script: 'aws sts get-caller-identity --query Account --output text', returnStdout: true).trim()
                    def ecrLogin = sh(script: "aws ecr get-login-password --region ${AWS_DEFAULT_REGION} | docker login --username AWS --password-stdin ${awsAccountId}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com", returnStatus: true)
                    docker.withRegistry("https://${awsAccountId}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com", 'aws-credentials') 
                    dockerImage.push("${env.BUILD_NUMBER}")
                    }
                }
            }
        }
    }
