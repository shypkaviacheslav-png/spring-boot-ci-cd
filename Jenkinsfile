pipeline {
    agent any

    environment {
        JAVA_HOME = '/usr/lib/jvm/java-11-openjdk-amd64'
        PATH = "${JAVA_HOME}/bin:${PATH}"
    }

    stages {
        stage('Check Java') {
            steps {
                sh 'java -version'
            }
        }

        stage('Build Code') {
            steps {
                sh 'gradle assemble'
                
                // --- МАГІЯ ТУТ ---
                // 1. Показуємо в логах, які файли ми створили (для перевірки)
                sh 'echo "Ось що лежить у папці build/libs:"'
                sh 'ls -la build/libs'

                // 2. Хитрість: Перейменовуємо знайдений .jar файл у той, який шукає Docker.
                // Ми шукаємо будь-який файл, що закінчується на .jar (окрім plain), і перейменовуємо його.
                sh 'mv build/libs/*SNAPSHOT.jar build/libs/spring-boot-ci-cd-0.0.1.jar || true'
                
                // Перевіряємо ще раз
                sh 'ls -la build/libs'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t my-lab-app .'
            }
        }

        stage('Deploy (Run)') {
            steps {
                sh 'docker stop my-running-app || true'
                sh 'docker rm my-running-app || true'
                sh 'docker run -d -p 8081:8080 --name my-running-app my-lab-app'
            }
        }
    }
}
