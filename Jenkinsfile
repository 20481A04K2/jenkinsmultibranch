pipeline {
    agent any

    environment {
        VENV_DIR = 'venv'
        WORK_DIR = "${env.WORKSPACE}"
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo "📦 Checking out code from multipipeline branch..."
                git branch: 'multipipeline', url: 'https://github.com/20481A04K2/jenkinsgcpvm.git'
            }
        }

        stage('Set up Python Virtual Environment') {
            steps {
                echo "🐍 Creating virtual environment..."
                sh '''
                    python3 -m venv ${VENV_DIR}
                    source ${VENV_DIR}/bin/activate
                '''
            }
        }

        stage('Install Requirements') {
            steps {
                echo "📦 Installing Python packages from requirements.txt..."
                sh '''
                    source ${VENV_DIR}/bin/activate
                    pip install --no-cache-dir -r requirements.txt
                '''
            }
        }

        stage('Restart Flask App') {
            steps {
                echo "🚀 Restarting Flask systemd service..."
                sh 'sudo systemctl restart flaskapp'
            }
        }

        stage('Check Flask Status') {
            steps {
                echo "📄 Checking Flask service status..."
                sh 'sudo systemctl status flaskapp || true'
            }
        }

        stage('Test App Response') {
            steps {
                echo "🌐 Checking if app is running via curl..."
                sh "curl -s http://<EXTERNAL_IP>:5000"
            }
        }
    }

    post {
        failure {
            echo "❌ Build failed!"
        }
        success {
            echo "✅ Deployment successful from multipipeline branch!"
        }
    }
}
