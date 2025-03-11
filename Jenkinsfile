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
                Start-Process -NoNewWindow -FilePath python -ArgumentList "app.py" -RedirectStandardOutput flask.log -RedirectStandardError flask_error.log
                Start-Sleep -Seconds 10
                '''
                echo 'Flask app started successfully!'
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
                Get-Process | Where-Object { $_.ProcessName -like "*python*" -and $_.MainWindowTitle -like "*Flask*" } | Stop-Process -Force
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Deployment Successful!'
        }
        failure {
            echo '❌ Deployment Failed. Check error logs.'
        }
    }
}
