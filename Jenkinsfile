pipeline {
    agent any
    environment {
        BUCKET = "magentomadrid-storage"
    }

    stages {
        stage('Stage to s3') {
            steps {
                withAWS(credentials: 'alabrador', region: 'eu-central-1') {
                    sh 'aws s3 sync . s3://$BUCKET --exclude ".git/*"'
                    sh 'aws s3 ls s3://$BUCKET'
                }
            }
        }
    }
}