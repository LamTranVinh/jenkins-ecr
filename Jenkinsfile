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
                sh 'printenv'
                sh 'docker build -t repo-jenkins .'
                }
            }
        stage('Test'){
            steps {
                 echo 'Empty'
            }
        }
        stage('Deploy') {
            steps {
                withEnv (["AWS_ACCESS_KEY_ID=${env.AWS_ACCESS_KEY_ID}", "AWS_SECRET_ACCESS_KEY_ID=${env.AWS_SECRET_ACCESS_KEY_ID}", "AWS_DEFAULT_REGION=${env.AWS_DEFAULT_REGION}"]) {
                    sh 'docker login -u AWS -p $(aws ecr-public get-login-password --region ap-northeast-2) 383468065570.dkr.ecr.ap-northeast-2.amazonaws.com'
                    sh 'docker build -t repo-jenkins .'
                    sh 'docker tag repo-jenkins:""$BUILD_ID""'
                    sh 'docker push 383468065570.dkr.ecr.ap-northeast-2.amazonaws.com/repo-jenkins:""$BUILD_ID""'
                    }
                }
            }
        }
    }
