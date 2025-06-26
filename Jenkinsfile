pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                echo "📦 Checking out code from main branch..."
                checkout scm
            }
        }

        stage('Set up Python Virtual Environment') {
            steps {
                sh '''
                    echo "🐍 Creating virtual environment..."
                    python3 -m venv venv
                    bash -c "source venv/bin/activate && echo ✅ Virtual environment ready"
                '''
            }
        }

        stage('Install Requirements') {
            steps {
                sh '''
                    bash -c "source venv/bin/activate && pip install -r requirements.txt"
                '''
            }
        }

        stage('Restart Flask App') {
            steps {
                echo "🚀 Restarting Flask app..."
                sh 'sudo systemctl restart flaskapp'
            }
        }

        stage('Check Flask Status') {
            steps {
                sh 'sudo systemctl status flaskapp || true'
            }
        }

        stage('Test App Response') {
            steps {
                sh 'curl http://35.201.247.151:5000'
            }
        }
    }

    post {
        success {
            echo '✅ Build and Deployment Successful!'
        }
        failure {
            echo '❌ Build failed!'
        }
    }
}
