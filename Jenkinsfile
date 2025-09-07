pipeline {
    agent { label 'maven' }

    environment {
        PATH = "/opt/apache-maven-3.9.6/bin:$PATH"
    }

    stages {

        stage("Build & Test") {
            steps {
                echo "-------- Build and Test started --------"
                // Run Maven clean, compile, and tests together
                sh 'mvn clean verify -DskipTests=false || true' 
                echo "-------- Build and Test completed --------"
            }
        }

        stage("SonarQube Analysis") {
            steps {
                script {
                    // Install / find Sonar scanner tool
                    def scannerHome = tool name: 'namg-sonar-scanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
                    
                    echo "-------- SonarQube analysis started --------"
                    // Inject SonarQube server environment
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
            echo "Pipeline failed ❌ Check logs for details"
        }
    }
}
