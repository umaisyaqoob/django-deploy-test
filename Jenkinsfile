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
                sh '''
                    python -m venv venv
                    source venv/bin/activate || venv\\Scripts\\activate
                    pip install --upgrade pip
                '''
            }
        }

        stage('Migrate Database') {
            steps {
                sh '''
                    source venv/bin/activate || venv\\Scripts\\activate
                    python manage.py migrate
                '''
            }
        }

        stage('Collect Static Files') {
            steps {
                sh '''
                    source venv/bin/activate || venv\\Scripts\\activate
                    python manage.py collectstatic --noinput
                '''
            }
        }

        stage('Run Development Server') {
            steps {
                sh '''
                    source venv/bin/activate || venv\\Scripts\\activate
                    nohup python manage.py runserver 0.0.0.0:8000 &
                '''
            }
        }
    }
}
