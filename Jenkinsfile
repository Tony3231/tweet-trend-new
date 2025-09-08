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

        stage("SonarQube Analysis") {
            steps {
                echo "🔎 Running SonarQube Analysis..."
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
                            error "❌ Quality Gate failed!"
                        } else {
                            echo "⚠️ Quality Gate status: ${qg.status} (not failing pipeline)"
                        }
                    }
                }
            }
        }
    }
}
