pipeline {
    agent any
    
    environment{
        
        NETLIFY_SITE_ID='552dee48-2a71-4d53-b9be-15d63b2fa732'
        Netlify_auth_token=credentials('Netlify-token')
    }


    stages {
        
       
        
        
        stage('Tests'){
            parallel{

                stage('Unit Test'){
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

            
                            post{
                                always {
                                    junit 'jest-results/junit.xml'
                                }


                            }


                        }

                stage('E2E'){
                    agent {
                        docker {
                            image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
                            reuseNode true
                        }
                    }
                    

                    steps{
                        sh '''
                        echo 'Small change3'
                        npm install  serve
                        node_modules/.bin/serve -s build &
                        sleep 10
                        npx playwright test --reporter=html
                        '''
                    }
                     post{
                        always {
                            publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, icon: '', keepAll: false, reportDir: 'playwright-report', reportFiles: 'index.html', reportName: 'Playwright HTML Report', reportTitles: '', useWrapperFileDirectly: true])
                        }


                    }


                 }
                }
        }

        stage('Deploy') {
        agent {
            docker {
                image 'node:18-alpine'
                reuseNode true
            }
        }

        steps {                    
            sh '''
            npm install netlify-cli
            node_modules/.bin/netlify --version
            echo "Deploying to production. Site ID: $NETLIFY_SITE_ID"
            node_modules/.bin/netlify deploy --dir=build --prod

            '''
        }
    }







    }


}

