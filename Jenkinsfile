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
            sh "docker build -t pycube-repo ."

            sh "docker tag pycube-repo:latest ${buildProps.AWS_ACCOUNT_ID}.dkr.ecr.us-east-1.amazonaws.com/pycube-repo:latest"

            sh "docker push ${buildProps.AWS_ACCOUNT_ID}.dkr.ecr.us-east-1.amazonaws.com/pycube-repo:latest"
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
