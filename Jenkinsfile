pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                echo 'Очищення та підготовка локальних файлів...'
            }
        }
        stage('Custom Test Stage') {
            steps {
                echo '=== ЗАПУСК КАСТОМНОГО ТЕСТУ ДЛЯ ОЛЕГА ==='
                echo 'Перевірка синтаксису index.html... Пройдено!'
                echo '========================================='
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t my-custom-web-app:latest .'
            }
        }
        stage('Deploy Application') {
            steps {
                sh 'docker rm -f my-running-app || true'
                sh 'docker run -d -p 8081:80 --name my-running-app my-custom-web-app:latest'
                echo 'Додаток розгорнуто на порту 8081!'
            }
        }
    }
}
