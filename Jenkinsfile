pipeline {
    agent any

    environment {
        // SonarCloud token stored in Jenkins credentials
        SONAR_TOKEN = credentials('sonar-credentials')
        JAVA_HOME = '/usr/lib/jvm/java-17-openjdk-amd64'
        PATH = "${JAVA_HOME}/bin:${env.PATH}"
    }

    options {
        // Keep only last 10 builds
        buildDiscarder(logRotator(numToKeepStr: '10'))
        // Timeout after 60 minutes
        timeout(time: 60, unit: 'MINUTES')
    }

    stages {

        stage('Checkout') {
            steps {
                echo "------ Checkout SCM ------"
                checkout scm
            }
        }

        stage('Build & Unit Tests') {
            steps {
                echo "------ Maven Build & Test ------"
                sh 'mvn clean install'
            }
            post {
                always {
                    echo "-------- Unit Test Completed --------"
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo "------- SonarQube Analysis Started --------"
                withSonarQubeEnv('namg-sonarqube-server') {
                    sh """
                        sonar-scanner \
                        -Dsonar.organization=namg04-key \
                        -Dsonar.projectKey=namg04-key_namtrend_${env.BRANCH_NAME} \
                        -Dsonar.projectName=Namtrend \
                        -Dsonar.sources=src/main/java \
                        -Dsonar.java.binaries=target/classes \
                        -Dsonar.coverage.jacoco.xmlReportPaths=target/site/jacoco/jacoco.xml \
                        -Dsonar.branch.name=${env.BRANCH_NAME} \
                        -Dsonar.login=${SONAR_TOKEN} \
                        -Dsonar.verbose=true
                    """
                }
            }
        }

        stage('Quality Gate') {
            steps {
                echo "------- Waiting for SonarQube Quality Gate -------"
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }

    post {
        success {
            echo "Pipeline finished SUCCESSFULLY for branch: ${env.BRANCH_NAME}"
        }
        failure {
            echo "Pipeline FAILED for branch: ${env.BRANCH_NAME}"
        }
    }
}
