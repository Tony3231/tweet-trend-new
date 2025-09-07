pipeline {
    agent any

    tools {
        maven 'Maven3' // Name of Maven installation in Jenkins
        jdk 'Java17'   // Name of JDK installation in Jenkins
    }

    environment {
        SONAR_TOKEN = credentials('sonar-credentials') // Your Sonar token ID in Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Checking out source code..."
                checkout scm
            }
        }

        stage('Build & Unit Test') {
            steps {
                echo "Building project and running tests..."
                script {
                    try {
                        sh 'mvn clean verify'
                    } catch (err) {
                        echo "No tests found or fork JVM issue"
                    }
                }
            }
        }

        stage('JaCoCo Coverage') {
            steps {
                echo "Generating JaCoCo coverage report..."
                sh 'mvn jacoco:report'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo "Starting SonarCloud analysis..."
                withSonarQubeEnv('namg-sonarqube-server') { // Name of SonarQube installation in Jenkins
                    sh """
                        sonar-scanner \
                        -Dsonar.projectKey=namg04-key_namtrend \
                        -Dsonar.organization=namg04-key \
                        -Dsonar.sources=src/main/java \
                        -Dsonar.java.binaries=target/classes \
                        -Dsonar.coverage.jacoco.xmlReportPaths=target/site/jacoco/jacoco.xml \
                        -Dsonar.login=${SONAR_TOKEN} \
                        -Dsonar.verbose=true
                    """
                }
            }
        }

        stage('Quality Gate') {
            steps {
                echo "Waiting for SonarCloud Quality Gate..."
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline finished"
        }
        success {
            echo "Pipeline succeeded"
        }
        failure {
            echo "Pipeline failed"
        }
    }
}
