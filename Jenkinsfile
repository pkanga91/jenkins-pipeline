pipeline {
   agent any

   environment {
    AWS_REGION = 'us-east-1'
    IMAGE_REPO = '864981748692.dkr.ecr.us-east-1.amazonaws.com/jenkins-ci'
    ECR_REPO = '864981748692.dkr.ecr.us-east-1.amazonaws.com'
   }
   stages{
    stage('CodeScan'){
        steps{
            sh 'trivy fs .  -o result.html'
            sh 'cat result.html'
           
        }
    }
    stage('dockerLogin'){
        steps{
            sh'aws ecr get-login-password --region $AWS_REGION | docker login --username AWS --password-stdin 864981748692.dkr.ecr.us-east-1.amazonaws.com'
        }
    }
    stage('dockerImageBuild'){
        steps{
            sh 'docker build -t jenkins-ci .'
            sh 'docker build -t imageversion . '
        }
    }
    stage('dockerImageTag'){
        steps{
            sh'docker tag jenkins-ci:latest 864981748692.dkr.ecr.us-east-1.amazonaws.com/jenkins-ci:latest'
            sh'docker tag imageversion 864981748692.dkr.ecr.us-east-1.amazonaws.com/jenkins-ci:v1.$BUILD_NUMBER'
        }    
    }
    stage('pushImage'){
        steps{
            sh 'docker push 864981748692.dkr.ecr.us-east-1.amazonaws.com/jenkins-ci:latest'
            sh 'docker push 864981748692.dkr.ecr.us-east-1.amazonaws.com/jenkins-ci:v1.$BUILD_NUMBER'
        }
    }
   } 
   }