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

        stage('Clean Workspace') {
            steps {
                script {
                    // Clean the workspace to remove any old build files
                    deleteDir()  // Cleans the workspace before starting fresh
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    // Install Poetry and set up the virtual environment
                    sh '''
                    # Install poetry
                    curl -sS https://install.python-poetry.org | python3 -

                    # Make sure Poetry is in the path
                    export PATH="$HOME/.local/bin:$PATH"
                    
                    # Install dependencies using Poetry (this will create a virtual environment automatically)
                    poetry install
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
                    sh 'gcloud app deploy --quiet'  // Use --quiet to suppress prompts
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
