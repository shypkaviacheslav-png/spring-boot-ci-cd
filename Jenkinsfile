pipeline {
    agent any
    environment {
        // Вказуємо шлях саме до 11-ї джави, яка вже встановлена у тебе
        JAVA_HOME = '/usr/lib/jvm/java-11-openjdk-amd64' 
        PATH = "${JAVA_HOME}/bin:${PATH}"
    triggers {
        pollSCM '* * * * *'
    }
    stages {
        stage('Build') {
            steps {
                sh 'gradle assemble'
            }
        }
         stage('Test') {
            steps {
                sh 'gradle test'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'gradle docker'
            }
        }
        stage('Run Docker Image') {
            steps {
                sh 'gradle dockerRun'
            }
        }
    }
}
