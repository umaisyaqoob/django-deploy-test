pipeline {
    agent any

    environment {
        PYTHONUNBUFFERED = 1
    }

    stages {
        stage('Clone Repo') {
            steps {
                git 'https://github.com/umaisyaqoob/django-deploy-test.git'
            }
        }

        stage('Set up Virtual Environment') {
            steps {
                bat '''
                    python -m venv venv
                    call venv\\Scripts\\activate
                    python -m pip install --upgrade pip
                '''
            }
        }

        stage('Install Requirements') {
            steps {
                bat '''
                    call venv\\Scripts\\activate
                    pip install -r requirements.txt
                '''
            }
        }

        stage('Migrate Database') {
            steps {
                bat '''
                    call venv\\Scripts\\activate
                    python manage.py migrate
                '''
            }
        }

        stage('Collect Static Files') {
            steps {
                bat '''
                    call venv\\Scripts\\activate
                    python manage.py collectstatic --noinput
                '''
            }
        }

        stage('Run Development Server') {
            steps {
                bat '''
                    call venv\\Scripts\\activate
                    start /b python manage.py runserver 0.0.0.0:8000
                '''
            }
        }
    }
}
