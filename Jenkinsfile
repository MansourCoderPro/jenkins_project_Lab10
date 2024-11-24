pipeline {
    agent any
    environment {
        VIRTUAL_ENV = 'venv'
    }
    stages {
        stage('Setup') {
            steps {
                script {
                    if (!fileExists("${env.WORKSPACE}/${VIRTUAL_ENV}")) {
                        bat "python -m venv ${VIRTUAL_ENV}"
                    }
                    bat "venv\\Scripts\\activate && pip install -r requirements.txt"
                }
            }
        }
        stage('Lint') {
            steps {
                script {
                    bat "venv\\Scripts\\activate && flake8 app.py"
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    bat "venv\\Scripts\\activate && pytest"
                }
            }
        }
        stage('Coverage') {
            steps {
                script {
                    bat "venv\\Scripts\\activate && coverage run -m pytest && coverage report"
                }
            }
        }
        stage('Security Scan') {
            steps {
                script {
                    bat """
                    set PYTHONIOENCODING=utf-8
                    venv\\Scripts\\activate
                    bandit -r . -f json -o bandit_report.json
                    """
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    echo "Deploying application..."
                }
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}
