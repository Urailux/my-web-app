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
        }
    }
    post {
        success {
            withCredentials([string(credentialsId: 'DISCORD_WEBHOOK_URL', variable: 'DISCORD_URL')]) {
                sh '''
                  curl -H "Content-Type: application/json" \
                       -X POST \
                       -d '{"content": "✅ Jenkins Build Successfully 🎉\\nJob: my-web-pipeline\\nStatus: SUCCESS"}' \
                       $DISCORD_URL
                '''
            }
        }
        failure {
            withCredentials([string(credentialsId: 'DISCORD_WEBHOOK_URL', variable: 'DISCORD_URL')]) {
                sh '''
                  curl -H "Content-Type: application/json" \
                       -X POST \
                       -d '{"content": "❌ Jenkins Build Fialed 😥\\nJob: my-web-pipeline\\nStatus: FAILURE"}' \
                       $DISCORD_URL
                '''
            }
        }
    }
}
