pipeline {
    agent any

    environment {
        // Set environment variables
        FIREBASE_CREDENTIALS = credentials('firebase-service-account') // Use Jenkins credential for Firebase service account
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the Git repository
                git 'https://github.com/Kamonchanok-tnd/Assignment5Cloud.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                // Install npm dependencies
                script {
                    sh 'npm install'
                }
            }
        }

        stage('Build') {
            steps {
                // Build the app (usually using npm run build or equivalent)
                script {
                    sh 'npm run build'
                }
            }
        }

        stage('Deploy to Firebase Hosting') {
            steps {
                // Deploy to Firebase Hosting using Firebase CLI
                script {
                    sh '''
                    # Authenticate using Firebase CLI
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
            // Clean up after the job is finished
            cleanWs()
        }
        success {
            // Notify success
            echo 'Deployment to Firebase Hosting was successful!'
        }
        failure {
            // Notify failure
            echo 'Deployment failed.'
        }
    }
}
