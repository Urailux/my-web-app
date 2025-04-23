pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/Urailux/my-web-app.git'
            }
        }
        stage('Build') {
            steps {
                echo 'No build needed for static website.'
            }
        }
        stage('Deploy') {
            steps {
                sh '''
                  mkdir -p /tmp/webdeploy
                  cp -r * /tmp/webdeploy/
                  echo "Website deployed to /tmp/webdeploy/"
                '''
            }
        }
    }
}

