pipeline {
    agent any
    environment {
        GOOGLE_APPLICATION_CREDENTIALS = credentials('google-service-account')
    }
    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }
        stage('Install Dependencies') {
            steps {
                script {
                    sh 'python3 -m venv /tmp/venv'
                    sh '. /tmp/venv/bin/activate && pip install --upgrade pip && pip install -r requirements.txt'
                }
            }
        }
        stage('Deploy to Google App Engine') {
            steps {
                script {
                    // Debug step to check Python version and environment
                    sh 'python --version'
                    sh 'gcloud --version'
                    sh 'gcloud auth activate-service-account --key-file=$GOOGLE_APPLICATION_CREDENTIALS'
                    sh 'gcloud config set project my-first-devops-project-444911'
                    
                    // Deploy with the correct path to app.yaml (in this case, the root of the project)
                    sh 'gcloud app deploy ./app.yaml --quiet'
                }
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}
