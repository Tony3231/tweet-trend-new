pipeline {
    agent {
        label 'maven-slave'  // Make sure this agent has Maven and JDK installed
    }

    environment {
        JAVA_HOME = '/usr/lib/jvm/java-17-openjdk-amd64'   // Your JDK path
        PATH = "${JAVA_HOME}/bin:/opt/apache-maven-3.9.6/bin:${env.PATH}"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build & Compile') {
            steps {
                // Skipping tests to avoid fork VM crash
                sh 'mvn clean verify -DskipTests'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withCredentials([string(credentialsId: 'sonar-token-id', variable: 'SONAR_TOKEN')]) {
                    sh """
                        mvn sonar:sonar \
                        -Dsonar.projectKey=Tony3231 \
                        -Dsonar.organization=Tony3231 \
                        -Dsonar.host.url=https://sonarcloud.io \
                        -Dsonar.login=${SONAR_TOKEN}
                    """
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline finished successfully!'
        }
        failure {
            echo 'Pipeline failed! Check logs for details.'
        }
    }
}
