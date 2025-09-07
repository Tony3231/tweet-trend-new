pipeline {
    agent any

    tools {
        jdk 'jdk17'         // Name of JDK installed in Jenkins global tool config
        maven 'Maven3'      // Name of Maven installed in Jenkins global tool config
    }

    environment {
        SONAR_TOKEN = credentials('sonar-token-id')  // Pulls token from Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build & Test') {
            steps {
                sh 'mvn clean verify'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                sh """
                    mvn sonar:sonar \
                      -Dsonar.projectKey=Tony3231_tweet-trend-new \
                      -Dsonar.organization=Tony3231 \
                      -Dsonar.host.url=https://sonarcloud.io \
                      -Dsonar.login=${SONAR_TOKEN}
                """
            }
        }
    }
}
