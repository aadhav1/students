
pipeline {
    agent any

    stages {

        stage('Git Checkout') {
            steps {
                echo 'Downloading Python project from Git'
                checkout scm
            }
        }

        stage('Check Python Version') {
            steps {
                sh '''
                    python3 --version
                    pip3 --version
                '''
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                    python3 -m venv venv
                    . venv/bin/activate
                    pip install -r requirements.txt
                '''
            }
        }

        stage('Run Unit Tests') {
            steps {
                sh '''
                    . venv/bin/activate
                    pytest -v --junitxml=test-results.xml
                '''
            }
        }

        stage('Run Python Program') {
            steps {
                sh '''
                    . venv/bin/activate
                    python3 calculator.py
                '''
            }
        }
    }

    post {
        always {
            junit allowEmptyResults: true,
                  testResults: 'test-results.xml'
        }

        success {
            echo 'Python application executed successfully!'
        }

        failure {
            echo 'Python pipeline failed. Check the logs.'
        }
    }
}
