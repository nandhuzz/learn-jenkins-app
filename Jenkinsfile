pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = '7df4cefe-0cb9-46f0-a12c-2337d394d74f'
        NETLIFY_AUTH_TOKEN = credentials('NETLIFY')
    }

    stages {

        stage('Build') {
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

       stage('Deploy stag') {
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
                    node_modules/.bin/netlify status
                    node_modules/.bin/netlify deploy --dir=build

                '''
            }
        }

        stage('Deploy prod') {
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
                    node_modules/.bin/netlify status
                    node_modules/.bin/netlify deploy --dir=build --prod

                '''
            }
        }
        
        // stage('Prod E2E') {
        //     agent {
        //         docker {
        //             image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
        //             reuseNode true
        //         }
        //     }

        //     environment {
        //         CI_ENVIRONMENT_URL = 'https://taupe-creponne-818a8c.netlify.app'
        //     }

        //     steps {
        //         sh '''
        //             npx playwright test  --reporter=html
        //         '''
        //     }

        //     post {
        //         always {
        //             publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'playwright-report', reportFiles: 'index.html', reportName: 'Playwright E2E', reportTitles: '', useWrapperFileDirectly: true])
        //         }
        //     }
        // }
    }
}