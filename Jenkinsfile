pipeline {
    agent { label 'maven-slave' }

    environment {
        SONAR_TOKEN = credentials('sonar-token-id')
    }

    tools {
        jdk 'jdk17'            // exact name from Global Tool Config
        maven 'Maven 3.9.6'    // exact name from Global Tool Config
    }

    stages {
        stage('Checkout') {
            steps { checkout scm }
        }

        stage('Build & Test') {
            steps {
                script {
                    def mvnHome = tool 'Maven 3.9.6'
                    sh "${mvnHome}/bin/mvn clean verify"
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('MySonarQube') {
                    script {
                        def mvnHome = tool 'Maven 3.9.6'
                        sh "${mvnHome}/bin/mvn sonar:sonar -Dsonar.login=${SONAR_TOKEN}"
                    }
                }
            }
        }
    }
}
