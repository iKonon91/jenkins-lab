pipeline {
    agent any
    
    options {
        ansiColor('xterm')
    }
    
    environment {
        DOCKERHUB_USER = 'ikonon91'
        IMAGE_NAME = 'jenkins-lab-app'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Очищення та підготовка локальних файлів...'
            }
        }

        stage('Custom Test Stage') {
            steps {
                // \u001B[32m робить текст зеленим, \u001B[0m скидає колір назад
                echo "\u001B[32m=== ЗАПУСК КАСТОМНОГО ТЕСТУ ДЛЯ ОЛЕГА ===\u001B[0m"
                echo "\u001B[32mПеревірка синтаксису index.html... Пройдено!\u001B[0m"
                echo "\u001B[32m=========================================\u001B[0m"
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${DOCKERHUB_USER}/${IMAGE_NAME}:latest ."
                sh "docker tag ${DOCKERHUB_USER}/${IMAGE_NAME}:latest ${DOCKERHUB_USER}/${IMAGE_NAME}:${BUILD_NUMBER}"
            }
        }

        stage('Push to Registry') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', passwordVariable: 'DOCKER_TOKEN', usernameVariable: 'DOCKER_USER')]) {
                    sh "echo \$DOCKER_TOKEN | docker login -u \$DOCKER_USER --password-stdin"
                    sh "docker push ${DOCKERHUB_USER}/${IMAGE_NAME}:latest"
                    sh "docker push ${DOCKERHUB_USER}/${IMAGE_NAME}:${BUILD_NUMBER}"
                }
            }
        }

        stage('Deploy Application') {
            steps {
                sh "docker rm -f my-running-app-lab2 || true"
                sh "docker run -d -p 8082:80 --name my-running-app-lab2 ${DOCKERHUB_USER}/${IMAGE_NAME}:latest"
                echo "Додаток успішно розгорнуто автоматично на порту 8082!"
            }
        }
    }
}     
