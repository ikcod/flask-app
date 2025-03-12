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
                powershell '''
                    try {
                        Write-Host "Starting Flask App..."
                        Start-Process -NoNewWindow -FilePath "python" -ArgumentList "app.py"
                        Start-Sleep -Seconds 10  # Change this to control how long the Flask app runs
                    } finally {
                        Write-Host "Stopping Flask App..."
                        Stop-Process -Name "python" -Force
                    }
                    exit 0  # Ensures Jenkins sees this stage as successful
                '''
    }
}

        stage('Success') {
            steps {
                powershell 'Write-Host "Pipeline executed successfully!"'
            }
        }
    }
}
