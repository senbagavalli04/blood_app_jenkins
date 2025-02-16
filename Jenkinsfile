pipeline {
    agent any

    environment {
        VENV = 'venv'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/senbagavalli04/blood_donation.git'
            }
        }

        stage('Setup Virtual Environment') {
            steps {
                sh '''
                python3 -m venv $VENV
                source $VENV/bin/activate
                pip install -r requirements.txt
                '''
            }
        }

        stage('Run Tests') {
            steps {
                sh '''
                source $VENV/bin/activate
                python manage.py test
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                source $VENV/bin/activate
                python manage.py migrate
                python manage.py runserver 0.0.0.0:8000 &
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Deployment Successful!'
            mail to: 'your-email@example.com',
                 subject: 'Jenkins Build Successful',
                 body: 'Your Blood Donation App has been deployed successfully!'
        }
        failure {
            echo '❌ Build Failed!'
        }
    }
}
