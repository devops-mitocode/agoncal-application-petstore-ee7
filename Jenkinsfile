pipeline {
    agent none
    environment {
        JBOSS_CREDENTIALS = credentials('jboss-credentials')
    }
    stages {
        stage('Build') {
            agent {
                docker {
                    image 'maven:3.8.8-eclipse-temurin-17-alpine'
                }
            }            
            steps {
                sh 'mvn clean package -B -ntp -DskipTests'
            }
        }
    }
}