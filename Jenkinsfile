pipeline {
    agent any

    environment {
        // Наша Java 11 для збірки
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
                // Збираємо .jar файл
                sh 'gradle assemble'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                // Створюємо контейнер (образ) з ім'ям 'my-lab-app'
                sh 'docker build -t my-lab-app .'
            }
        }

        stage('Deploy (Run)') {
            steps {
                // 1. Зупиняємо старий контейнер (якщо він був), щоб не було конфлікту імен. 
                // "|| true" означає: "якщо помилка (контейнера нема) - не зупиняй збірку"
                sh 'docker stop my-running-app || true'
                sh 'docker rm my-running-app || true'
                
                // 2. Запускаємо новий.
                // -d : у фоновому режимі
                // -p 8081:8080 : Порт 8080 (внутрішній) прокидаємо на 8081 (зовнішній), 
                // бо на 8080 вже сидить сам Jenkins!
                sh 'docker run -d -p 8081:8080 --name my-running-app my-lab-app'
            }
        }
    }
}
