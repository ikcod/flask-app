pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                powershell 'Write-Host "Building the Flask App..."'
            }
        }
        stage('Run Flask App') {
            steps {
                powershell 'Start-Process -NoNewWindow -FilePath "python" -ArgumentList "app.py"'
                powershell 'Write-Host "Flask app started successfully!"'
            }
        }
        stage('Success') {
            steps {
                powershell 'Write-Host "Pipeline executed successfully!"'
            }
        }
    }
}
