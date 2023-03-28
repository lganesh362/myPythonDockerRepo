pipeline {
    agent any
  

    stages{
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
    
       stage('Read Env Variable') {
            steps {
                script {
                    ls
                    }
                }
            }
        }
       

    }
