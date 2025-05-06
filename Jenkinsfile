pipeline {
    agent any

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
            // Move the cleanWs() inside node block
            node {
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
