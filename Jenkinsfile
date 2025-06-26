pipeline {
    agent any

    environment {
        APP_DIR = "/var/lib/jenkins/jenkinsgcpvm"
        VENV_DIR = "${APP_DIR}/venv"
        PYTHON = "${VENV_DIR}/bin/python"
    }

    stages {
        stage('Clone Repo') {
            steps {
                echo "📥 Cloning code to $APP_DIR"
                sh '''
                    rm -rf $APP_DIR
                    git clone https://github.com/20481A04K2/jenkinsmultibranch.git $APP_DIR
                '''
            }
        }

        stage('Set up Virtualenv') {
            steps {
                sh '''
                    cd $APP_DIR
                    if [ ! -d "$VENV_DIR" ]; then
                        python3 -m venv $VENV_DIR
                    fi
                    $PYTHON -m pip install --upgrade pip
                    $PYTHON -m pip install -r requirements.txt
                '''
            }
        }

        stage('Kill Previous App') {
            steps {
                sh 'pkill -f app.py || true'
            }
        }

        stage('Prepare Log') {
            steps {
                sh '''
                    cd $APP_DIR
                    sudo rm -f app.log
                    sudo touch app.log
                    sudo chmod 666 app.log
                    sudo chown sajja_vamsi:sajja_vamsi app.log
                '''
            }
        }

        stage('Run Flask App') {
            steps {
                sh '''
                    cd $APP_DIR
                    nohup $PYTHON app.py > app.log 2>&1 &
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Flask app deployed successfully!"
        }
        failure {
            echo "❌ Build failed. Check logs!"
        }
    }
}
