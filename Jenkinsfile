pipeline {
    agent { label 'maven-slave' }  // or the agent you use

    environment {
        JAVA_HOME = tool name: 'jdk17', type: 'jdk'          // exact JDK name in Jenkins
        PATH = "${env.JAVA_HOME}/bin:${tool 'Maven3'}/bin:${env.PATH}"  // exact Maven name in Jenkins
        SONAR_TOKEN = credentials('sonar-token-id')          // Jenkins credential ID for Sonar token
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Tony3231/tweet-trend-new.git',
                    credentialsId: 'Github_credentials'
            }
        }

        stage('Build & Test') {
            steps {
                sh 'mvn clean verify'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarCloud') { // name of your SonarCloud server in Jenkins
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

    post {
        always {
            echo 'Pipeline finished.'
        }
        failure {
            mail to: 'your-email@example.com',
                 subject: "Pipeline Failed: ${currentBuild.fullDisplayName}",
                 body: "Check Jenkins console output: ${env.BUILD_URL}"
        }
    }
}
