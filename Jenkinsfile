pipeline {
    agent { label 'maven' }

    environment {
        PATH = "/opt/apache-maven-3.9.11/bin:$PATH"
    }

    stages {
        stage("Build") {
            steps {
                echo "🚀 Starting Maven Build..."
                sh 'mvn clean install -DskipTests'
                echo "✅ Build Completed"
            }
        }

        stage("SonarCloud Analysis") {
            steps {
                echo "🔎 Running SonarCloud Analysis..."
                withSonarQubeEnv('sonar-server') {   // Must match Jenkins → Configure System
                    withCredentials([string(credentialsId: 'sonarqube-key', variable: 'SONAR_TOKEN')]) {
                        sh """
                            mvn sonar:sonar \
                              -Dsonar.projectKey=tony888_tony \
                              -Dsonar.organization=tony888 \
                              -Dsonar.host.url=https://sonarcloud.io \
                              -Dsonar.login=$SONAR_TOKEN
                        """
                    }
                }
            }
        }

        stage("Quality Gate") {
            steps {
                echo "⏳ Waiting for SonarCloud Quality Gate result..."
                timeout(time: 1, unit: 'HOURS') {
                    script {
                        def qg = waitForQualityGate()
                        if (qg.status == 'OK') {
                            echo "✅ Quality Gate passed!"
                        } else if (qg.status == 'ERROR') {
                            echo "❌ Quality Gate failed, marking build UNSTABLE."
                            currentBuild.result = 'UNSTABLE'
                        } else {
                            echo "⚠️ Quality Gate status is ${qg.status}, continuing without failure."
                        }
                    }
                }
            }
        }
    }

    post {
        always {
            echo "🔹 Build finished with status: ${currentBuild.currentResult}"
        }
    }
}
