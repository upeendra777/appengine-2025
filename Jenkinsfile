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
                    // Install Python and pip locally (without sudo)
                    sh '''
                    # Install python3-venv and pip
                    curl -sS https://install.python-poetry.org | python3 -
                    export PATH="$HOME/.local/bin:$PATH"
                    
                    # Create a virtual environment
                    python3 -m venv /tmp/venv
                    
                    # Activate the virtual environment and upgrade pip
                    . /tmp/venv/bin/activate
                    pip install --upgrade pip
                    
                    # Install the dependencies from requirements.txt
                    pip install -r requirements.txt
                    '''
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    // Ensure virtual environment is activated and pytest is installed
                    sh '''
                    # Activate the virtual environment
                    . /tmp/venv/bin/activate
                    
                    # Install pytest explicitly if it's not already in requirements.txt
                    pip install pytest
                    
                    # Run tests using pytest via python -m
                    python -m pytest
                    '''
                }
            }
        }

        stage('Deploy to Google App Engine') {
            steps {
                script {
                    // Authenticate with Google Cloud
                    sh 'gcloud auth activate-service-account --key-file=${GOOGLE_APPLICATION_CREDENTIALS}'

                    // Set project ID
                    sh 'gcloud config set project $PROJECT_ID'

                    // Deploy to App Engine
                    sh 'gcloud app browse'
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
            // Optional: Clean up after the pipeline
        }

        success {
            echo 'Deployment successful!'
        }

        failure {
            echo 'Deployment failed!'
        }
    }
}
