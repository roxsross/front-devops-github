pipeline {
    agent any
    environment{
        BUCKET = "jenkins-web-2023"
    }

    stages {
        stage('deploy to s3') {
            steps {
                withAWS(credentials: 'aws-roxsross', region: 'us-east-1') {
                    sh 'aws s3 sync . s3://$BUCKET'
                    sh 'aws s3 ls'
             }
           }
        }
    }
}
