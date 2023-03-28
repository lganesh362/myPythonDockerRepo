pipeline {
    agent any
  
    stages{
          stage('Initialize')
    {
        def dockerHome = tool 'docker'
        def mavenHome  = tool 'maven'
        env.PATH = "${dockerHome}/bin:${mavenHome}/bin:${env.PATH}"
    }
       stage('Check Env Variable') {
            steps {
                script {
                        buildProps = readProperties file: 'build.properties'
                //printing all build properties
                //echo "Property file is : ${buildProps}"
                //getting all build properties into variables
                project_key = "${buildProps.project_key}"
                echo "${buildProps.BRANCH_NAME}"
                echo "${buildProps.CRED}"
                echo "${buildProps.git_url}"
                }
              }
          }
       stage ('cloneing')
       {

            steps {
                git branch: buildProps.BRANCH_NAME, credentialsId: buildProps.CRED, url: buildProps.git_url
            }
        }
        stage('Push Docker image to ECR') {
            steps {
            withAWS(credentials: 'aws-credentials', region: 'us-east-1') {
            sh '''docker build -t pycube-repo .

            docker tag pycube-repo:latest 686509451139.dkr.ecr.us-east-1.amazonaws.com/pycube-repo:latest

            docker push 686509451139.dkr.ecr.us-east-1.amazonaws.com/pycube-repo:latest'''
        }
    }
}
    
   
        }
       

    }
