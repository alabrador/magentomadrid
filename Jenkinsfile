pipeline {
    agent any
    environment {
        BUCKET = "magentomadrid-storage"
    }

    stages {
        stage('Init'){
            agent {
                docker {
                    image 'node:alpine'
                    args '-u root:root'
                }
            }
            steps {
                sh 'npm install'
            }
        }
        stage('Test'){
            steps {
                sh 'npm run test'
            }
        }
        stage('Build'){
            agent {
                docker {
                    image 'node:alpine'
                    args '-u root:root'
                }
            }
            steps {
                sh 'npm run build'
                stash includes: 'dist/**/**', name: 'dist'
            }
        }
        stage('Deployment AWS s3') {
            steps {
                withAWS(credentials: 'aws-alabrador', region: 'eu-central-1') {
                    sh 'aws s3 sync . s3://$BUCKET --exclude ".git"'
                    sh 'aws s3 ls s3://$BUCKET'
                }
            }

        }
    }
}