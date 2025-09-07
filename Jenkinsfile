pipeline {
    agent { label 'maven' }

    environment {
        PATH = "/opt/apache-maven-3.9.6/bin:$PATH"
    }

    stages {

        stage("Build") {
            steps {
                echo "-------- Build started --------"
                sh 'mvn clean deploy -Dmaven.test.skip=true'
                echo "-------- Build completed --------"
            }
        }

        stage("Unit Test") {
            steps {
                echo "-------- Unit test started --------"
                sh 'mvn surefire-report:report'
                echo "-------- Unit test completed --------"
            }
        }

        stage("SonarQube Analysis") {
            steps {
                script {
                    // Attempt to find the SonarQube Scanner tool
                    def scannerHome = ''
                    try {
                        scannerHome = tool name: 'namg-sonar-scanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
                    } catch (err) {
                        error "SonarQube Scanner tool 'namg-sonar-scanner' not found! Configure it in Jenkins Global Tool Configuration."
                    }

                    echo "-------- SonarQube analysis started --------"
                    withSonarQubeEnv('namg-sonarqube-server') {
                        sh """
                            ${scannerHome}/bin/sonar-scanner \
                            -Dsonar.projectKey=tweet-trend \
                            -Dsonar.sources=src \
                            -Dsonar.java.binaries=target/classes
                        """
                    }
                    echo "-------- SonarQube analysis completed --------"
                }
            }
        }

        stage("Quality Gate") {
            steps {
                echo "-------- Waiting for SonarQube Quality Gate --------"
                timeout(time: 1, unit: 'HOURS') {
                    waitForQualityGate abortPipeline: true
                }
                echo "-------- Quality Gate passed --------"
            }
        }
    }

    post {
        success {
            echo "Pipeline completed successfully ✅"
        }
        failure {
            echo "Pipeline failed ❌"
        }
    }
}
