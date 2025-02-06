pipeline {
    agent any
    environment {
        BUCKET = "magentomadrid-storage"
    }

    stages {
        stage('Deployment AWS s3') {
            steps {
                withAWS(credentials: 'aws-alabrador', region: 'eu-central-1') {
                    sh 'aws s3 sync . s3://$BUCKET'
                    sh 'aws s3 ls s3://$BUCKET'
                }
            }
        }
    }
}