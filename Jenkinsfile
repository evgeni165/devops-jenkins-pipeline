pipeline {
    agent any

    environment {
        APP_ENV = 'staging'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
                sh 'ls -la'
            }
        }

        stage('Build') {
            steps {
                echo 'Сборка приложения...'
                sh 'echo Build completed'
            }
        }

        stage('Test') {
            steps {
                echo 'Тестирование приложения...'
                sh './smoke-tests'
            }
        }

        stage('Deploy to Staging') {
            steps {
                echo 'Деплой на staging...'
                sh './deploy staging'
            }
        }

        stage('Smoke Test Staging') {
            steps {
                echo 'Проверка staging...'
                sh './smoke-tests'
            }
        }

        stage('Approval for Production') {
            steps {
                input message: 'Разрешить деплой в production?', ok: 'Deploy'
            }
        }

        stage('Deploy to Production') {
            steps {
                echo 'Деплой на production...'
                sh './deploy production'
            }
        }
    }

    post {
        always {
            echo 'Очистка рабочей области...'
            deleteDir()
        }
        success {
            echo 'Pipeline успешно завершён'
        }
        failure {
            echo 'Pipeline завершился с ошибкой'
        }
    }
}
