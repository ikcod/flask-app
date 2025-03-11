pipeline {
    agent any

    environment {
        VENV = "venv"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/ikcod/flask-app.git'
            }
        }

        stage('Setup Python Environment') {
            steps {
                powershell '''
                python --version
                python -m venv $env:VENV
                '''
            }
        }

        stage('Install Dependencies') {
            steps {
                powershell '''
                . $env:VENV\\Scripts\\Activate
                pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }

        stage('Run Flask Application (Background)') {
            steps {
                powershell '''
                . $env:VENV\\Scripts\\Activate
                Start-Process -NoNewWindow -FilePath python -ArgumentList "app.py"
                Start-Sleep -Seconds 5
                '''
            }
        }

        stage('Run Tests') {
            steps {
                powershell '''
                . $env:VENV\\Scripts\\Activate
                pytest tests/
                '''
            }
        }

        stage('Stop Flask') {
            steps {
                powershell '''
                Get-Process | Where-Object { $_.ProcessName -like "*python*" } | Stop-Process -Force
                '''
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
