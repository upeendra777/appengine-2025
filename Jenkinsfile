pipeline {
    agent any

    environment {
        PROJECT_ID = 'my-first-devops-project-444911'
        GOOGLE_APPLICATION_CREDENTIALS = credentials('gcp-service-account')  // Service account credential
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/saleemafroze/appengine-2025.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    // Install dependencies without requiring sudo password
                    sh '''
                    # Update apt package list
                    sudo apt-get update -y
                    
                    # Install pip3 and Python development tools if not installed
                    sudo apt-get install -y python3-pip python3-dev
                    
                    # Upgrade pip to the latest version with --break-system-packages
                    sudo pip3 install --upgrade pip --break-system-packages
                    
                    # Install dependencies from requirements.txt
                    sudo pip3 install -r requirements.txt
                    '''
                }
            }
        }

        stage('Deploy to Google App Engine') {
            steps {
                script {
                    // Ensure gcloud is installed and show working directory
                    sh 'gcloud --version'
                    sh 'pwd'  // Print the working directory
                    sh 'ls -l'  // List contents to ensure app.yaml is present

                    // Authenticate with Google Cloud using service account
                    sh 'gcloud auth activate-service-account --key-file=${GOOGLE_APPLICATION_CREDENTIALS}'

                    // Set the project ID for Google Cloud
                    sh 'gcloud config set project $PROJECT_ID'

                    // Deploy the application to App Engine
                    sh 'gcloud app deploy app.yaml --quiet'  // Quiet flag prevents interactive prompts
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
            cleanWs()  // Optional: Clean workspace after the pipeline
        }

        success {
            echo 'Deployment successful!'
        }

        failure {
            echo 'Deployment failed!'
        }
    }
}
