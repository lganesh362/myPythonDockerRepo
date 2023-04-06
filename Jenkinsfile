pipeline {
    agent any
  

    stages{
       stage('Check Env Variable') {
            steps {
                script {
                        buildProps = readProperties file: 'build.properties'
                echo "${buildProps.BRANCH_NAME}"
                echo "${buildProps.CRED}"
                echo "${buildProps.git_url}"
                echo "${buildProps.AWS_ACCOUNT_ID}"
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
            sh '''aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 686509451139.dkr.ecr.us-east-1.amazonaws.com'''
                
            sh '''docker build -t pycube-repo:(${env.BUILD_NUMBER}) .'''

            sh '''docker tag pycube-repo:(${env.BUILD_NUMBER}) ${buildProps.AWS_ACCOUNT_ID}.dkr.ecr.us-east-1.amazonaws.com/pycube-repo:(${env.BUILD_NUMBER})'''

            sh '''docker push ${buildProps.AWS_ACCOUNT_ID}.dkr.ecr.us-east-1.amazonaws.com/pycube-repo:(${env.BUILD_NUMBER})'''
        }
    }
}

        stage('Pull Docker image from ECR') {
            steps {
            withAWS(credentials: 'aws-credentials', region: 'us-east-1') {
            sh "docker pull ${buildProps.AWS_ACCOUNT_ID}.dkr.ecr.us-east-1.amazonaws.com/pycube-repo:latest"
            sh "docker rm -f pythonapp"
            sh "docker run -itd -p 5000:5000 --name pythonapp ${buildProps.AWS_ACCOUNT_ID}.dkr.ecr.us-east-1.amazonaws.com/pycube-repo:latest"

        }
    }
}
    
   
        }
       

    }
