pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'pip3 install -r requirements.txt'
            }
        }
        stage('Test') {
            steps {
                sh 'python3 manage.py test'
            }
        }
        stage('Deploy to staging') {
            steps {
                sh 'ssh -o StrictHostKeyChecking=no deployment-user@192.168.56.7 "source venv/bin/activate; \
                cd polling; \
                git pull origin main; \
                pip install -r requirements.txt --no-warn-script-location; \
                python manage.py migrate; \
                deactivate; \
                sudo systemctl restart nginx; \
                sudo systemctl restart gunicorn "'
            }
        }
        stage('Deploy to prod') {
            steps {
                sh 'ssh -o StrictHostKeyChecking=no deployment-user@192.168.56.4 "source venv/bin/activate; \
                cd polling; \
                git pull origin main; \
                pip install -r requirements.txt --no-warn-script-location; \
                python manage.py migrate; \
                deactivate; \
                sudo systemctl restart nginx; \
                sudo systemctl restart gunicorn "'
            }
        }
    }
}