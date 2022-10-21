pipeline {
  agent any
  stages {
    stage ('Build') {
      steps {
        sh 'printenv'
      }
    }
    stage ('Publish to ECR') {
      steps {
         withEnv(["AWS_ACCESS_KEY_ID=${env.AWS_ACCESS_KEY_ID}", "AWS_SECRET_ACCESS_KEY=${env.AWS_SECRET_ACCESS_KEY}", "AWS_DEFAULT_REGION=${env.AWS_DEFAULT_REGION}"]) {
          sh 'docker login -u AWS -p $(aws ecr get-login-password --region eu-west-2) 927491280662.dkr.ecr.eu-west-2.amazonaws.com' 
          sh 'docker build -t python-pipeline .'
          sh 'docker tag python-pipeline:latest 927491280662.dkr.ecr.eu-west-2.amazonaws.com/python-pipeline:latest'
          sh 'docker push 927491280662.dkr.ecr.eu-west-2.amazonaws.com/python-pipeline:latest'
         }
       }
    }
  }
}
