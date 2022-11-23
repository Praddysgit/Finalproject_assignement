@Library('github.com/releaseworks/jenkinslib') _

pipeline {
    agent any
    environment {
        registry = "803187127260.dkr.ecr.us-east-1.amazonaws.com/finalproject"
    }

    stages {
        stage('Cloning Git') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '', url: 'https://github.com/Praddysgit/Finalproject_assignement.git']]])
            }
        }

    // Uploading Docker images into AWS ECR
    stage('Pushing to ECR') {
        steps{
            script {
                sh 'docker build -t 803187127260.dkr.ecr.us-east-1.amazonaws.com/finalproject . '
                sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 803187127260.dkr.ecr.us-east-1.amazonaws.com '
                sh 'docker push 803187127260.dkr.ecr.us-east-1.amazonaws.com/finalproject'
            }
        }
    }

    
    stage('Docker Run') {
     steps{
         script {
             sshagent(credentials : ["63384ps9-aef1-474a-9c07-b63fg99sbwoa"]){

                sh 'ssh -t -t ubuntu@10.0.1.154 -o StrictHostKeyChecking=no "docker login -u AWS -p $(aws ecr get-login-password --region us-east-1) 803187127260.dkr.ecr.us-east-1.amazonaws.com && docker run -d -p 8080:8081 --rm --name nodeapp 803187127260.dkr.ecr.us-east-1.amazonaws.com/finalproject:latest"'

             }
                
                
            }
      }
    }
    }
}
