pipeline {
    agent any

    environment {
        // Sets up our isolated virtual environment chamber on the cloud machine
        VENV_PATH = "${WORKSPACE}/venv"
    }

    stages {
        stage('Build Environment') {
            steps {
                echo 'Step 1: Setting up an isolated Python environment sandbox...'
                sh "python3 -m venv ${VENV_PATH}"
                sh "${VENV_PATH}/bin/pip install --upgrade pip"
                sh "${VENV_PATH}/bin/pip install -r requirements.txt"
            }
        }

        stage('Test Suite') {
            steps {
                echo 'Step 2: Executing testing framework configurations...'
                sh "${VENV_PATH}/bin/pytest"
            }
        }

        stage('Deploy to Staging') {
            steps {
                echo 'Step 3: Launching your Flask app live to Port 5000...'
                // Clear any old process running on Port 5000 so the application doesn't crash
                sh "fuser -k 5000/tcp || true"
                // Run the app securely in the cloud server background
                sh "BUILD_ID=dontKillMe nohup ${VENV_PATH}/bin/flask run --host=0.0.0.0 --port=5000 > flask.log 2>&1 &"
            }
        }
    }
}
