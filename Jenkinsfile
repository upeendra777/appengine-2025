pipeline {
    agent any

    environment {
        GOOGLE_CLOUD_PROJECT = 'my-first-devops-project-444911'  // Your GCP project ID
        GOOGLE_CLOUD_REGION = 'us-central'  // Your GCP region (you can adjust it)
        GOOGLE_APPLICATION_CREDENTIALS = '/path/to/your/service-account-key.json'  // Service account key for authentication
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Checkout code from your repository
                git 'https://github.com/your-repo/your-project.git'
            }
        }

        stage('Set up Google Cloud SDK') {
            steps {
                script {
                    // Install Google Cloud SDK
                    sh 'curl https://sdk.cloud.google.com | bash'
                    sh 'source $HOME/google-cloud-sdk/path.bash.inc'
                }
            }
        }

        stage('Authenticate with Google Cloud') {
            steps {
                script {
                    // Authenticate with Google Cloud using your service account key
                    sh 'gcloud auth activate-service-account --key-file=$GOOGLE_APPLICATION_CREDENTIALS'
                }
            }
        }

        stage('Set Google Cloud Project') {
            steps {
                script {
                    // Set the project ID for gcloud
                    sh "gcloud config set project ${GOOGLE_CLOUD_PROJECT}"
                }
            }
        }

        stage('Deploy to App Engine') {
            steps {
                script {
                    // Deploy to Google App Engine
                    sh 'gcloud app deploy --quiet'
                }
            }
        }
    }

    post {
        always {
            // Clean up or notify on failure
            echo 'Pipeline finished.'
        }
    }
}
