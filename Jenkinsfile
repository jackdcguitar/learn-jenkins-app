pipeline {
    agent any
    
     environment {
        REACT_APP_VERSION = "1.0.$BUILD_ID"
        AWS_DEFAULT_REGION = 'us-east-1'
    }


  
    stages {
        
          
        stage('Deply to  AWS'){
            agent{
                docker{
                    image 'amazon/aws-cli' 
                    reuseNode true
                    args '-u root --entrypoint=""'   
                }
            }
            

          
            
            steps{
                withCredentials([usernamePassword(credentialsId: 'my-aws', passwordVariable: 'AWS_SECRET_ACCESS_KEY', usernameVariable: 'AWS_ACCESS_KEY_ID')]) {
                    sh '''
                        aws --version
                        yum install jq -y
                    
                        LATEST_TD_REVISION=$(aws ecs register-task-definition --cli-input-json file://aws/task-definition-prod.json |jq '.taskDefinition')
                        echo $LATEST_TD_REVISION

                        aws ecs update-service --cluster LearnJenkinsApp-Cluster-Prod --service LearnJenkinsApp-Service-Prod --task-definition LearnJenkinsApp-TaskDefinition-Prod
                        aws ecs wait services-stable --cluster LearnJenkinsApp-Cluster-Prod --services LearnJenkinsApp-Service-Prod                                            '''
                }
            }
        }


        stage('Docker') {
            steps {
                sh 'docker build -t my-playwright .'
            }
        }

        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                }
            }
            steps {
                sh '''
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }
      
       


      
    }
}