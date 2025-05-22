pipeline {
    agent any

    environment {
        SSH_KEY = credentials('SSH_KEY')  // your Jenkins SSH credential ID
        REMOTE_USER = "ubuntu"
        REMOTE_HOST = "139.99.101.104"
        REMOTE_PATH = "/home/ubuntu/django-deploy-test"
    }

    stages {

        stage('Prepare Python Environment') {
            steps {
                bat '''
                    python -m venv venv
                    call venv\\Scripts\\activate
                    pip install --upgrade pip
                    pip install -r requirements.txt
                '''
            }
        }

        stage('Run Migrations') {
            steps {
                bat '''
                    call venv\\Scripts\\activate
                    python manage.py migrate
                '''
            }
        }

        stage('Fix SSH Key Permissions') {
            steps {
                bat '''
                    echo Fixing SSH key permissions...
                    icacls "%SSH_KEY%" /inheritance:r
                    icacls "%SSH_KEY%" /grant:r "%USERNAME%:R"
                '''
            }
        }

        stage('Deploy to VPS') {
            steps {
                bat '''
                    echo Deploying code to remote server...
                    scp -i "%SSH_KEY%" -r . %REMOTE_USER%@%REMOTE_HOST%:%REMOTE_PATH%
                '''
            }
        }
    }
}
