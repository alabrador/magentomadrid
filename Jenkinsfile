pipeline {
    agent any
    environment {
        BUCKET = "magentomadrid-storage"
        CLOUDFRONT_DISTRIBUTION_ID = "E2JS7FZJJAOLB3"
        AWS_REGION = "eu-central-1"
    }

    stages {
        stage('Instalar Dependencias') {
            steps {
                script {
                    sh 'npm install'
                }
            }
        }
        stage('Construir Proyecto Astro') {
            steps {
                script {
                    sh 'npm run build'
                }
            }
        }
        stage('Subir proyecto AWS s3') {
            steps {
                withAWS(credentials: 'aws-alabrador', region: 'eu-central-1') {
                    sh 'aws s3 sync . s3://$BUCKET --delete --exclude ".git"'
                    sh 'aws s3 ls s3://$BUCKET'
                }
            }

        }
        stage('Invalidar Cache CloudFront') {
            steps {
                script {
                    withAWS(credentials: 'aws-alabrador', region: 'eu-central-1') {
                        sh 'aws cloudfront create-invalidation --distribution-id ${CLOUDFRONT_DISTRIBUTION_ID} --paths "/*" --region ${AWS_REGION}'
                    }
                }
            }
        }
    }
}