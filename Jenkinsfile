pipeline {
    agent any
    stages {
        /*   

        stage('Build') {
            // This is a comment
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
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
        */
        stage('E2E'){
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.55.0-noble'
                    reuseNode true
                }
            }
            steps{
                sh '''
                     npm install -g serve
                     serve -s build
                     npx playwright test
                '''
            }
        }
    }
           agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps{
                sh '''
                    #test -f build/index.html
                    npm test
                '''
            }
        }


    

    post{
        always{
            junit 'test-results/junit.xml'
        }
    }

