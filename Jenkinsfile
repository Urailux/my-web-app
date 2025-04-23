pipeline {
    agent any
    environment {
        NOW = ""
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
        stage('Notify Discord') {
            steps {
                script {
                    def now = sh(script: "date '+%Y-%m-%d %H:%M'", returnStdout: true).trim()
                    def status = currentBuild.currentResult
                    def emoji = (status == 'SUCCESS') ? '✅' : '❌'
                    def message = "${emoji} Jenkins Build ${status == 'SUCCESS' ? 'SUCCESS' : 'Failed'} \\n📅 DateTime: ${now}\\n📦 Job: ${env.JOB_NAME}\\n📄 Status: ${status}"

                    withCredentials([string(credentialsId: 'DISCORD_WEBHOOK_URL', variable: 'DISCORD_URL')]) {
                        sh """
                          curl -H "Content-Type: application/json" \
                               -X POST \
                               -d '{\"content\": \"${message}\"}' \
                               \$DISCORD_URL
                        """
                    }
                }
            }
        }
    }
}
