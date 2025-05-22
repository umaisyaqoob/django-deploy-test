pipeline {
    agent any
    environment {
        SSH_KEY = credentials('live-server-ssh')
        REMOTE_USER = "ubuntu"
        REMOTE_HOST = "139.99.101.104"
        REMOTE_PATH = "/home/ubuntu/django-deploy-test"
    }
    stages {
        stage('Deploy to Live Server') {
            steps {
                sshagent(['live-server-ssh']) {
                    sh """
                    ssh -o StrictHostKeyChecking=no $REMOTE_USER@$REMOTE_HOST '
                        cd $REMOTE_PATH &&
                        git pull &&
                        source venv/bin/activate &&
                        pip install -r requirements.txt &&
                        python manage.py migrate &&
                        sudo systemctl restart gunicorn
                    '
                    """
                }
            }
        }
    }
}
