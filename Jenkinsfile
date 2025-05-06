pipeline {
    agent any  // หรือสามารถกำหนดเป็น agent ที่ต้องการให้รัน pipeline นี้

    environment {
        FIREBASE_CREDENTIALS = credentials('firebase-service-account') // Firebase Service Account Key
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/Kamonchanok-tnd/Assignment5Cloud.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    sh 'npm install'
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    sh 'npm run build'
                }
            }
        }

        stage('Deploy to Firebase Hosting') {
            steps {
                script {
                    sh '''
                    echo $FIREBASE_CREDENTIALS > firebase-credentials.json
                    firebase use --add your-firebase-project-id
                    firebase deploy --token $(firebase login:ci --token $(cat firebase-credentials.json)) --only hosting
                    '''
                }
            }
        }
    }

    post {
        always {
            node('master') {  // ระบุ label 'master' หรือ 'any' ถ้าใช้ Jenkins master node
                cleanWs()
            }
        }
        success {
            echo 'Deployment to Firebase Hosting was successful!'
        }
        failure {
            echo 'Deployment failed.'
        }
    }
}
