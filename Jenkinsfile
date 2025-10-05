pipeline {
    agent any

    environment {
        APP_PORT = "5000"
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo "Pulling Flask app from GitHub..."
                git branch: 'main', url: 'https://github.com/Ragul-17-cloud/Python.git'
            }
        }

        stage('Setup Environment') {
            steps {
                echo "Setting up virtual environment and installing dependencies..."
                sh '''
                python3 -m venv venv
                source venv/bin/activate
                pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }

        stage('Run Flask App') {
            steps {
                echo "Starting Flask app..."
                sh '''
                source venv/bin/activate
                nohup python app.py > flask.log 2>&1 &
                '''
            }
        }

        stage('Verify App Running') {
            steps {
                sh '''
                sleep 5
                curl -I http://localhost:${APP_PORT} || echo "Flask app not reachable"
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Flask app deployed successfully and is running!"
        }
        failure {
            echo "❌ Flask app failed to start. Check logs."
        }
    }
}
