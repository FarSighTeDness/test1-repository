pipeline {
    agent any

    options {
        timestamps()
    }

    environment {
        PYTHONUNBUFFERED = '1'
        PIP_DISABLE_PIP_VERSION_CHECK = '1'
        IMAGE_NAME = 'django-todo-cicd'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Setup Python') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'python --version'
                        sh 'python -m pip install --upgrade pip'
                        sh 'python -m pip install django==3.2'
                    } else {
                        bat 'python --version'
                        bat 'python -m pip install --upgrade pip'
                        bat 'python -m pip install django==3.2'
                    }
                }
            }
        }

        stage('Migrate') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'python manage.py migrate'
                    } else {
                        bat 'python manage.py migrate'
                    }
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'python manage.py test'
                    } else {
                        bat 'python manage.py test'
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} .'
                    } else {
                        bat 'docker build -t %IMAGE_NAME%:%BUILD_NUMBER% .'
                    }
                }
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'db.sqlite3', allowEmptyArchive: true
        }
    }
}
