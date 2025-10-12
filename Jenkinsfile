pipeline {
    agent any
    
    environment {
       
    }

    stages {
        
          
        stage('Deply to  AWS'){
            agent{
                docker{
                    image 'amazon/aws-cli' 
                    reuseNode true
                    args '--entrypoint=""'   
                }
            }
            
            environment{
                


            }
            
            steps{
                withCredentials([usernamePassword(credentialsId: 'my-aws', passwordVariable: 'AWS_SECRET_ACCESS_KEY', usernameVariable: 'AWS_ACCESS_KEY_ID')]) {
                    sh '''
                        aws --version
                        aws ecs register-task-definition file://aws/task-definition-prod.json
                    '''
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