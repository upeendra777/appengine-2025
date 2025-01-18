pipeline {
    agent any

    environment {
        PROJECT_ID = 'my-first-devops-project-444911'
        GOOGLE_APPLICATION_CREDENTIALS = credentials('gcp-service-account')  // Service account credential
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/saleemafroze/appengine-poc.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    // Check if requirements.txt exists and install dependencies globally
                    if (fileExists('requirements.txt')) {
                        // Install dependencies globally using pip
                        sh '''
                        # Upgrade pip and install dependencies globally
                        pip3 install --upgrade pip
                        pip3 install -r requirements.txt
                        '''
                    } else {
                        echo "requirements.txt not found!"
                        exit 1
                    }
                }
            }
        }

        stage('Deploy to Google App Engine') {
            steps {
                script {
                    // Check if gcloud is installed and show current working directory
                    sh 'gcloud --version'
                    sh 'pwd'  // Print working directory
                    sh 'ls -l'  // List contents to ensure app.yaml is present

                    // Authenticate with Google Cloud
                    sh 'gcloud auth activate-service-account --key-file=${GOOGLE_APPLICATION_CREDENTIALS}'

                    // Set project ID
                    sh 'gcloud config set project $PROJECT_ID'

                    // Deploy to App Engine (with the correct runtime)
                    sh 'gcloud app deploy app.yaml --quiet'  // Removed the ./ from the path
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
            cleanWs()  // Optional: clean workspace after the pipeline
        }

        success {
            echo 'Deployment successful!'
        }

        failure {
            echo 'Deployment failed!'
        }
    }
}
