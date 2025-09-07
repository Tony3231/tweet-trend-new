pipeline {
    agent any
    tools {
        maven 'Maven3'  // must match the name you configured
        jdk 'OpenJDK17' // must match the name of your JDK in Jenkins
    }
    environment {
        SONAR_TOKEN = credentials('sonar-token-id') // your Sonar token
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
