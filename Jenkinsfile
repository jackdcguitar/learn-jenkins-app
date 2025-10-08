pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }

            steps {                    // ✅ 只有一個 steps
                sh '''
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -la
                '''
                echo 'Hello World'     // ✅ 合併到同一個 steps
            }
        }

        stage('Test'){
            steps{
                sh 'test -f build/index.html'
            }

        }

    }
}