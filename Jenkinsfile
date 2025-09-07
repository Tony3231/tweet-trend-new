pipeline {
    agent any
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
            environment {
                SONAR_TOKEN = credentials('sonar-credentials')
            }
            steps {
                withSonarQubeEnv('namg-sonarqube-server') {
                    sh """
                    sonar-scanner \
                    -Dsonar.projectKey=namg04-key_namtrend_${env.BRANCH_NAME} \
                    -Dsonar.branch.name=${env.BRANCH_NAME} \
                    -Dsonar.organization=namg04-key \
                    -Dsonar.sources=src/main/java \
                    -Dsonar.java.binaries=target/classes \
                    -Dsonar.coverage.jacoco.xmlReportPaths=target/site/jacoco/jacoco.xml \
                    -Dsonar.projectName=Namtrend
                    """
                }
            }
        }
    }
    post {
        always {
            echo "Pipeline finished!"
        }
    }
}
