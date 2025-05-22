pipeline {
  agent any

  environment {
    PYTHONUNBUFFERED = '1'
  }

  stages {
    stage('Clone Repo') {
      steps {
        git 'https://github.com/umaisyaqoob/django-deploy-test.git'
      }
    }

    stage('Build & Migrate on Jenkins') {
      steps {
        bat '''
          python -m venv venv
          call venv\\Scripts\\activate
          python -m pip install --upgrade pip
          call venv\\Scripts\\activate
          pip install -r requirements.txt
          call venv\\Scripts\\activate
          python manage.py migrate
        '''
      }
    }

    stage('Deploy to VPS') {
      steps {
        withCredentials([sshUserPrivateKey(
          credentialsId: 'live-server-ssh',
          keyFileVariable: 'SSH_KEY',
          usernameVariable: 'SSH_USER'
        )]) {
          bat """
            ssh -i %SSH_KEY% -o StrictHostKeyChecking=no -o StrictModes=no %SSH_USER%@139.99.101.104 ^
              "cd ~/django-deploy-test && \
               git pull origin master && \
               source venv/bin/activate && \
               pip install -r requirements.txt && \
               python manage.py migrate && \
               sudo systemctl restart django-app"
          """
        }
      }
    }
  }
}
