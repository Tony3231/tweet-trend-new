pipeline {
    agent any
    tools {
        jdk 'Java17'        // <-- exact name from Jenkins Global Tool Configuration
        maven 'Maven3'      // <-- exact name from Jenkins if configured
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
