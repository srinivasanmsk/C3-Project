#!/usr/bin/env groovy

pipeline {

    // Running the pipeline in the App Host by configuring the App Host as "Agent"
    agent { label 'worker1' }
    
    stages {      
        stage('Cloning Git') {
            steps {
                git url: 'https://github.com/srinivasanmsk/C3-Project', branch: "main" 
            }
        }
  
    // Building Docker images
    stage('Building image') {
      steps{
        sh "sudo docker build . -t 828839113893.dkr.ecr.us-east-1.amazonaws.com/c3-assignment:${BUILD_NUMBER}"
      }
    }
    
   
    // Uploading Docker images into AWS ECR
    stage('Pushing to ECR') {
     steps{  
        sh "sudo aws ecr get-login-password --region us-east-1 | sudo docker login --username AWS --password-stdin 828839113893.dkr.ecr.us-east-1.amazonaws.com"
        sh "sudo docker push 828839113893.dkr.ecr.us-east-1.amazonaws.com/c3-assignment:${BUILD_NUMBER}"
        }
      }

    // Runs the container in App Host by checking whether there are any running containers with the name "node-app".
    // IF YES, stops the container, remove it and run the container with the latest image
    // ELSE, run the container directly
    stage('Running the container in App Host') {
     steps{  
        sh '''if [ $(sudo docker ps -a | tail -n 1 | grep node-app | wc -l) -gt 0 ]; then
              sudo docker stop node-app
              sudo docker rm -f node-app
              fi '''
        sh "sudo docker run -itd -p 8080:8080 --name node-app 828839113893.dkr.ecr.us-east-1.amazonaws.com/c3-assignment:${BUILD_NUMBER}"
        }
      }
    }
}
