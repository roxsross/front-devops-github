pipeline {
    agent any
    environment{
        BUCKET = "jenkins-web-2023"
        BOT_URL="https://api.telegram.org/bot5881753165:AAEjB95ZRDUW0kRMCzMA7C1yjpHemiGTpiM/sendMessage"
        TELEGRAM_CHAT_ID="-1001508340482"
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
    }//end stages
    post { 
        success { 
            sh  ("""
                curl -s -X POST $BOT_URL -d chat_id=${TELEGRAM_CHAT_ID} -d parse_mode=markdown -d text='*Full project name*: ${env.JOB_NAME} \n*Branch*: [$GIT_BRANCH]($GIT_URL) \n*Build* : [OK](${BUILD_URL}consoleFull)'
            """)
         }
         failure {
            sh  ("""
                curl -s -X POST $BOT_URL -d chat_id=${TELEGRAM_CHAT_ID} -d parse_mode=markdown -d text='*Full project name*: ${env.JOB_NAME} \n*Branch*: [$GIT_BRANCH]($GIT_URL) \n*Build* : [Not OK](${BUILD_URL}consoleFull)'
            """)
         }
    }
}
}
