pipeline {
    agent any

    environment {
        VENV_DIR = 'venv'
    }

    stages {
        stage('Clone Repository') {
            steps {
                sh '''
                    echo "Pulling latest code from Git repository..."
                    if [ -d "app" ]; then
                        cd app && git pull
                    else
                        git clone https://github.com/ikcod/flask-app.git app && cd app
                    fi
                '''
            }
        }

        stage('Setup Environment') {
            steps {
                sh '''
                    echo "Setting up Python environment..."
                    python3 -m venv $VENV_DIR
                    source $VENV_DIR/bin/activate
                    pip install --upgrade pip
                    pip install -r requirements.txt
                '''
            }
        }

        stage('Build') {
            steps {
                sh 'echo "Building the Flask App..."'
            }
        }

        stage('Run and Deploy Flask App') {
            steps {
                sh '''
                    echo "Starting Flask App..."
                    source $VENV_DIR/bin/activate
                    nohup python3 app.py &> flask_app.log &
                    FLASK_PID=$!
                    sleep 10
                    if ps -p $FLASK_PID > /dev/null
                    then
                        echo "Flask App is running."
                    else
                        echo "Failed to start Flask App." >&2
                        exit 1
                    fi

                    echo "Stopping Flask App..."
                    kill -9 $FLASK_PID
                '''
            }
        }

        stage('Success') {
            steps {
                sh 'echo "Pipeline executed successfully!"'
            }
        }
    }

    post {
        always {
            sh 'echo "Cleaning up environment..."'
            sh 'rm -rf $VENV_DIR'
        }
    }
}
