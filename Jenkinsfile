pipeline {
    agent any
    environment {
        NOW = "" // สำหรับเก็บวันที่และเวลา
    }
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
        always {
            script {
                // บันทึกวันที่-เวลา เช่น "2025-04-23 18:45"
                env.NOW = sh(script: "date '+%Y-%m-%d %H:%M'", returnStdout: true).trim()
            }
        }
        success {
            withCredentials([string(credentialsId: 'DISCORD_WEBHOOK_URL', variable: 'DISCORD_URL')]) {
                sh '''
                  curl -H "Content-Type: application/json" \
                       -X POST \
                       -d "{\"content\": \"✅ Jenkins Build successfully 🎉\\n📅 เวลา: ${NOW}\\n📦 Job: my-web-pipeline\\n🟢 Status: SUCCESS\"}" \
                       $DISCORD_URL
                '''
            }
        }
        failure {
            withCredentials([string(credentialsId: 'DISCORD_WEBHOOK_URL', variable: 'DISCORD_URL')]) {
                sh '''
                  curl -H "Content-Type: application/json" \
                       -X POST \
                       -d "{\"content\": \"❌ Jenkins Build Failed 😥\\n📅 เวลา: ${NOW}\\n📦 Job: my-web-pipeline\\n🔴 Status: FAILURE\"}" \
                       $DISCORD_URL
                '''
            }
        }
    }
}
