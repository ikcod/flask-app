pipeline {
    agent any

    environment {
        VENV = 'venv'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/ikcod/flask-app.git'
            }
        }

        stage('Setup Python Environment') {
            steps {
                powershell 'python --version'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'source $VENV/bin/activate && pip install -r requirements.txt'
            }
        }

        stage('Run Flask Application') {
            steps {
                echo 'Starting Flask App...'
                sh 'source $VENV/bin/activate && python app.py &'
            }
        }
    }

    post {
        success {
            echo 'Deployment Successful!'
        }
        failure {
            echo 'Deployment Failed. Check error logs.'
        }
    }
}
