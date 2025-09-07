pipeline {
    agent any
    tools {
        jdk 'jdk17'       // <-- exact name from Jenkins
        maven 'Maven3'    // <-- exact name from Jenkins
    }
    environment {
        SONAR_TOKEN = credentials('sonar-token-id') 
    }
    stages {
        stage('Build & Test') {
            steps {
                sh 'mvn clean verify'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('namg-sonarqube-server') {
                    sh 'mvn sonar:sonar -Dsonar.login=$SONAR_TOKEN'
                }
            }
        }
    }
}
