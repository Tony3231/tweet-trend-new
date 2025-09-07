pipeline {
    agent { label 'maven-slave' }

    tools {
        maven 'Maven 3.9.6'  // must match the name in Global Tool Config
        jdk 'jdk17'           // must match your JDK name
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('SonarQube Analysis') {
            environment {
                SONAR_TOKEN = credentials('sonar-token-id')
            }
            steps {
                sh 'mvn sonar:sonar -Dsonar.login=$SONAR_TOKEN'
            }
        }
    }
}
