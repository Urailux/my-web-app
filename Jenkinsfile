pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Urailux/my-web-app.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Lint') {
            steps {
                sh 'npm run lint'
            }
        }
        stage('Test') {
            steps {
                sh 'npm run test'
            }
        }
        stage('Deploy') {
            steps {
                sh '''
                  mkdir -p /tmp/webdeploy
                  cp -r * /tmp/webdeploy/
                  echo "✅ Deployed to /tmp/webdeploy/"
                '''
            }
stage('Notify Discord') {
    steps {
        withCredentials([string(credentialsId: 'DISCORD_WEBHOOK_URL', variable: 'DISCORD_URL')]) {
            sh '''
              curl -H "Content-Type: application/json" \
                   -X POST \
                   -d '{"content": "✅ Jenkins Build สำเร็จแล้วค่ะพี่!"}' \
                   $DISCORD_URL
            '''
        }
    }
}
    }
}

