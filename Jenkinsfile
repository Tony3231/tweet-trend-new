pipeline {
    agent { label 'maven-slave' }

    environment {
        // Point to your JDK and Maven installation on the agent
        JAVA_HOME = '/usr/lib/jvm/java-17-openjdk-amd64'
        PATH = "${JAVA_HOME}/bin:/opt/apache-maven-3.9.6/bin:${env.PATH}"
        SONAR_TOKEN = credentials('sonar-token-id') // Jenkins credentials
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
                    -Dsonar.projectKey=namg04-key_namtrend \
                    -Dsonar.organization=Tony3231 \
                    -Dsonar.host.url=https://sonarcloud.io \
                    -Dsonar.login=${SONAR_TOKEN} \
                    -Dsonar.java.binaries=target/classes
                """
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the logs.'
        }
    }
}
