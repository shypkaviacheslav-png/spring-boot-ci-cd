pipeline {
    agent any

    environment {
        // Примусово використовуємо Java 11 для збірки
        JAVA_HOME = '/usr/lib/jvm/java-11-openjdk-amd64'
        PATH = "${JAVA_HOME}/bin:${PATH}"
    }

    stages {
        stage('Check Java Version') {
            steps {
                // Цей крок покаже нам в логах, чи підтягнулась Java 11
                sh 'java -version'
            }
        }

        stage('Build') {
            steps {
                // Збираємо проект
                sh 'gradle assemble'
            }
        }
        
        stage('Test') {
            steps {
                // Запускаємо тести (якщо є)
                sh 'gradle test'
            }
        }
    }
}
