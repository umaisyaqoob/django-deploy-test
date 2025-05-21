pipeline {
    agent any

    environment {
        PYTHON = 'python3'
        VENV = 'venv'
    }

    stages {

        stage('Clone Repo') {
            steps {
                git 'https://github.com/umaisyaqoob/django-deploy-test.git'
            }
        }

        stage('Setup Virtual Environment') {
            steps {
                sh '''
                    $PYTHON -m venv $VENV
                    source $VENV/bin/activate
                    pip install --upgrade pip
                    pip install django
                '''
            }
        }

        stage('Run Migrations') {
            steps {
                sh '''
                    source $VENV/bin/activate
                    python manage.py migrate
                '''
            }
        }

        stage('Run Django Server') {
            steps {
                sh '''
                    source $VENV/bin/activate
                    nohup python manage.py runserver 0.0.0.0:8000 &
                '''
            }
        }
    }

    post {
        success {
            echo '🎉 Django app deployed and running!'
        }
        failure {
            echo '❌ Build failed. Please check logs.'
        }
    }
}
