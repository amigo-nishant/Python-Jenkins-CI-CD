pipeline {
  agent any
   parameters {
  choice choices: ['Main', 'DEV', 'PROD'], name: 'Branch'
}
    stages{
    stage('SCM checkout'){
    steps{
    echo "Pulling changes from the branch ${params.Branch}"
    git url: 'https://github.com/amigo-nishant/Python-hello-world.git', branch: "${params.Branch}"
    }
  }
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
    stage ('Deploy to EC2') {
      steps{
               sh 'ssh -o StrictHostKeyChecking=no -i /var/lib/jenkins/python-dev.pem ec2-user@ec2-13-41-226-172.eu-west-2.compute.amazonaws.com aws ecr get-login-password --region eu-west-2 | docker login --username AWS --password-stdin 927491280662.dkr.ecr.eu-west-2.amazonaws.com'
               sh 'ssh -o StrictHostKeyChecking=no -i /var/lib/jenkins/python-dev.pem ec2-user@ec2-13-41-226-172.eu-west-2.compute.amazonaws.com sudo docker run -d -p 5002:5000 927491280662.dkr.ecr.eu-west-2.amazonaws.com/python-pipeline:latest'
         }
       }
    }
 }

  
 
  

