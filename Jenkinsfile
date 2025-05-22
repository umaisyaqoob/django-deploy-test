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

        stage('Run Development Server') {
            steps {
                bat '''
                    call venv\\Scripts\\activate
                    start /b python manage.py runserver 0.0.0.0:8000
                '''
            }
        }

        stage('Deploy to Live Server') {
            steps {
                sshagent(credentials: ['live-server-ssh']) {
                    sh '''
                        ssh -o StrictHostKeyChecking=no ubuntu@139.99.101.104 '
                            cd /home/ubuntu/django-deploy-test &&
                            git pull origin main &&
                            source venv/bin/activate &&
                            pip install -r requirements.txt &&
                            python manage.py migrate &&
                            sudo systemctl restart django-app
                        '
                    '''
                }
            }
        }
    }
}
